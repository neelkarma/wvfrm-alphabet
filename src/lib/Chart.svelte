<script lang="ts">
  import {
    Chart,
    BarController,
    BarElement,
    LinearScale,
    CategoryScale,
    ScatterController,
    PointElement,
    Legend,
    Tooltip,
  } from "chart.js";

  interface Props {
    times: number[];
    mistakes: number[];
  }

  let { times, mistakes }: Props = $props();

  let canvas: HTMLCanvasElement | undefined = $state();

  $effect(() => {
    if (!canvas) return;

    const data = [];

    for (let i = 0; i < 26; i++) {
      data.push({
        letter: String.fromCharCode(i + 97).toUpperCase(),
        time: times[i] - (times[i - 1] ?? 0),
        mistakes: mistakes[i],
      });
    }

    Chart.register(
      BarController,
      BarElement,
      CategoryScale,
      LinearScale,
      ScatterController,
      PointElement,
      Legend,
      Tooltip,
    );

    Chart.defaults.color = "#abb2bf";
    Chart.defaults.borderColor = "#3e3e40";
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
            backgroundColor: "#61afef",
          },
          {
            type: "scatter",
            label: "Errors",
            pointStyle: "crossRot",
            yAxisID: "errors",
            order: 0,
            data: data.map((row) => row.mistakes),
            borderWidth: 2,
            borderColor: "#e06c75",

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

<canvas bind:this={canvas} style="width: 100%;"></canvas>
