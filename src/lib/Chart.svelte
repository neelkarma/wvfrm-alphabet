<script lang="ts">
  import Chart from "chart.js/auto";
  import { onMount } from "svelte";

  export let times: number[];
  export let mistakes: number[];

  let canvas: HTMLCanvasElement;
  onMount(() => {
    const data = [];

    for (let i = 0; i < 26; i++) {
      data.push({
        letter: String.fromCharCode(i + 97).toUpperCase(),
        time: times[i] - (times[i - 1] ?? 0),
        mistakes: mistakes[i],
      });
    }

    Chart.defaults.backgroundColor = "#00000000";
    Chart.defaults.borderColor = "#303030";
    Chart.defaults.color = "#c0c0c0";
    Chart.defaults.font.family = "JetBrains Mono";
    new Chart(canvas, {
      type: "bar",
      data: {
        labels: data.map((row) => row.letter),
        datasets: [
          {
            type: "bar",
            label: "Time",
            yAxisID: "time",
            order: 1,
            data: data.map((row) => row.time),
          },
          {
            type: "scatter",
            label: "Errors",
            pointStyle: "crossRot",
            yAxisID: "errors",
            order: 0,
            data: data.map((row) => row.mistakes),
            borderWidth: 2,

            // Hide points if there were no mistakes
            pointRadius: function (context): number {
              const index = context.dataIndex;
              const value = context.dataset.data[index] as number;
              return (value ?? 0) <= 0 ? 0 : 5;
            },
            pointHoverRadius: function (context): number {
              const index = context.dataIndex;
              const value = context.dataset.data[index] as number;
              return (value ?? 0) <= 0 ? 0 : 8;
            },
          },
        ],
      },
      options: {
        scales: {
          x: {
            axis: "x",
            display: true,
            title: {
              display: true,
              text: "Letter",
            },
          },
          time: {
            axis: "y",
            display: true,
            beginAtZero: true,
            title: {
              display: true,
              text: "Time (ms)",
            },
          },
          errors: {
            axis: "y",
            display: true,
            position: "right",
            title: {
              display: true,
              text: "Errors",
            },
            beginAtZero: true,
            ticks: {
              precision: 0,
            },
            grid: { display: false },
          },
        },
      },
    });
  });
</script>

<canvas bind:this={canvas} style="width: 100%;" />
