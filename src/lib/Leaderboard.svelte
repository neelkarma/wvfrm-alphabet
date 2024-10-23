<script lang="ts">
  interface Props {
    bestTime: number;
  }

  let { bestTime }: Props = $props();

  let leaderboard = $derived(
    [
      {
        name: "Tom Scott",
        time: 3555,
        url: "https://www.youtube.com/@TomScottGo",
      },
      {
        name: "Cleo Abram",
        time: 4266,
        url: "https://www.youtube.com/@CleoAbram",
      },
      {
        name: "Quinn (Snazzy Labs)",
        time: 4432,
        url: "https://www.youtube.com/@snazzy",
      },
      { name: "Marques", time: 4539, url: "https://www.youtube.com/@mkbhd" },
      {
        name: "Josh Wardle",
        time: 4973,
        url: "https://twitter.com/powerlanguish",
      },
      {
        name: "Jad Abumrad",
        time: 5176,
        url: "https://twitter.com/jadabumrad",
      },
      { name: "Adam", time: 5205, url: "https://twitter.com/adamlukas17" },
      { name: "Andrew", time: 5249, url: "https://twitter.com/AndyManganelli" },
      {
        name: "Doug Demuro",
        time: 5900,
        url: "https://www.youtube.com/@DougDeMuro",
      },
      {
        name: "Dr. Mike",
        time: 5970,
        url: "https://www.youtube.com/@HealthyGamerGG",
      },
      {
        name: "SoKrispyMedia",
        time: 6259,
        url: "https://www.youtube.com/@SoKrispyMedia",
      },
      { name: "Hayato", time: 6537, url: "https://twitter.com/hayatohuseman" },
      {
        name: "JerryRigEverything",
        time: 6627,
        url: "https://www.youtube.com/@JerryRigEverything",
      },
      {
        name: "Mark Rober",
        time: 6633,
        url: "https://www.youtube.com/@MarkRober",
      },
      {
        name: "Austin Evans",
        time: 6676,
        url: "https://www.youtube.com/@austinevans",
      },
      {
        name: "Colin + Samir",
        time: 6677,
        url: "https://www.youtube.com/@ColinandSamir",
      },
      {
        name: "Simone Giertz",
        time: 7009,
        url: "https://www.youtube.com/@simonegiertz",
      },
      {
        name: "Hank Green",
        time: 7200,
        url: "https://www.youtube.com/@vlogbrothers",
      },
      {
        name: "Vinh",
        time: 7392,
        url: "https://twitter.com/danggvinh?lang=zh-Hant",
      },
      { name: "Michael", time: 7569 },
      { name: "David", time: 8364, url: "https://www.youtube.com/@DavidImel" },
      {
        name: "Tim",
        time: 8530,
        url: "https://twitter.com/timmcmahonn?lang=en",
      },
      {
        name: "Hasan",
        time: 8778,
        url: "https://www.youtube.com/@HasanMinhaj",
      },
      {
        name: "David Blaine",
        time: 8821,
        url: "https://www.youtube.com/@DavidBlaine",
      },
      { name: "Brandon", time: 9442 },
      { name: "You", time: bestTime },
    ].sort(({ time: a }, { time: b }) => a - b),
  );
</script>

<p>Leaderboard:</p>
<table>
  <thead>
    <tr>
      <th>#</th>
      <th>Name</th>
      <th>Time (sec)</th>
    </tr>
  </thead>
  <tbody>
    {#each leaderboard as { name, time, url }, i (i)}
      <tr>
        <td>{i + 1}</td>
        <td class:you={name === "You"}>
          {#if url}
            <a href={url}>{name}</a>
          {:else}
            {name}
          {/if}
        </td>
        <td>{(time / 1000).toFixed(3)}</td>
      </tr>
    {/each}
  </tbody>
</table>

<style>
  table {
    max-width: 100%;
  }

  th:nth-child(2) {
    width: 100%;
  }

  th:not(:nth-child(2)) {
    text-wrap: nowrap;
  }

  th {
    text-align: left;
  }

  th,
  td {
    padding: 6px;
  }

  td.you {
    font-weight: bold;
    color: #ffffff;
  }

  tr:not(:first-child) {
    border-top: 1px solid #404040;
  }

  tr > td:first-child {
    color: #a0a0a0;
  }

  tr:nth-child(2) > td:first-child {
    font-weight: bold;
    color: #ffd700;
  }
  tr:nth-child(3) > td:first-child {
    font-weight: bold;
    color: #ffffff;
  }
  tr:nth-child(4) > td:first-child {
    font-weight: bold;
    color: #cd7f32;
  }
</style>
