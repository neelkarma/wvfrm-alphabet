<script lang="ts">
  import Alphabet from "$lib/Alphabet.svelte";
  import Chart from "$lib/Chart.svelte";
  import HiddenInput from "$lib/HiddenInput.svelte";
  import Leaderboard from "$lib/Leaderboard.svelte";
  import { onMount } from "svelte";
  import type { KeyboardEventHandler } from "svelte/elements";

  let input: HiddenInput;
  let alphabet: Alphabet;

  onMount(() => {
    input.focus();
  });

  let state: "unstarted" | "running" | "ended" = "unstarted";
  let currentLetterIndex = 0;
  let times = Array(26).fill(0);
  let mistakes = Array(26).fill(0);

  let interval = -1;
  let startTime = Date.now();
  let bestTime = Infinity;
  let timeElapsed = 0;

  const updateTime = () => {
    timeElapsed = Date.now() - startTime;
  };

  const reset = () => {
    state = "unstarted";
    currentLetterIndex = 0;
    timeElapsed = 0;
    input?.focus();
    times.fill(0);
    mistakes.fill(0);
  };

  const start = () => {
    state = "running";
    startTime = Date.now();
    interval = setInterval(updateTime);
  };

  const end = () => {
    state = "ended";
    input?.blur();
    clearInterval(interval);
  };

  const handleInput = (e: CustomEvent<string>) => {
    if (state === "ended") return;
    if (state === "unstarted") start();

    const typed = e.detail;
    const currentLetter = String.fromCharCode(currentLetterIndex + 97);

    if (typed === currentLetter) {
      times[currentLetterIndex] = timeElapsed;
      currentLetterIndex += 1;
      if (currentLetterIndex >= 26) {
        if (timeElapsed < bestTime) bestTime = timeElapsed;
        end();
      }
    } else {
      mistakes[currentLetterIndex] += 1;
      alphabet.triggerIncorrect();
    }
  };

  const handleKeyDown: KeyboardEventHandler<Window> = (e) => {
    if (!["Tab", "Enter"].includes(e.key)) return;
    e.preventDefault();
    end();
    reset();
  };
</script>

<svelte:window on:keydown={handleKeyDown} />

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<div on:click={() => input.focus()}>
  <p>
    See how fast you can type the alphabet! Start typing to start, and press Tab
    or Enter to reset.
  </p>

  <Alphabet {currentLetterIndex} bind:this={alphabet} />
  <p class="timer">
    {#if state === "unstarted"}
      0.000 sec
    {:else}
      {(timeElapsed / 1000).toFixed(3)} sec
    {/if}
  </p>
  <p>
    Best Time: {#if bestTime === Infinity}Unset{:else}{(
        bestTime / 1000
      ).toFixed(3)} sec{/if}
  </p>
  <HiddenInput bind:this={input} on:input={handleInput} />
  {#if state === "ended"}
    {@const totalMistakes = mistakes.reduce((a, b) => a + b, 0)}
    <p>
      Total Mistakes: {totalMistakes}
      {#if totalMistakes === 0}(flawless!){/if}
    </p>
    <Chart {times} {mistakes} />
    <Leaderboard {bestTime} />
    <a href="https://www.youtube.com/@Waveform">Follow the Waveform podcast!</a>
    <p class="credits">
      Made with &lt;3 by <a href="https://iamkneel.vercel.app/">iamkneel</a>
    </p>
  {/if}
</div>

<style>
  div {
    display: flex;
    flex-direction: column;
    gap: 8px;
  }

  .timer {
    font-size: 1.5rem;
  }

  .credits {
    font-size: 0.8rem;
    color: #a0a0a0;
  }
</style>
