<script lang="ts">
  interface Props {
    currentLetterIndex: number;
  }

  let { currentLetterIndex }: Props = $props();

  let currentLetterEl: HTMLSpanElement | undefined = $state();

  export const triggerIncorrect = () => {
    currentLetterEl?.animate(
      [{ backgroundColor: "#ff3333", color: "#ffffff" }, {}],
      {
        duration: 500,
        easing: "cubic-bezier(0, 0, 0.2, 1)",
      },
    );
  };

  let letters = $derived(
    Array.from(Array(26)).map((_, i) => {
      let state: string;

      if (i > currentLetterIndex) state = "untyped";
      else if (i < currentLetterIndex) state = "typed";
      else state = "current";

      return [String.fromCharCode(i + 97), state];
    }),
  );
</script>

<div class="container">
  {#each letters as [letter, state] (letter)}
    {#if state === "current"}
      <span class="letter" data-state={state} bind:this={currentLetterEl}
        >{letter.toUpperCase()}</span
      >
    {:else}
      <span class="letter" data-state={state}>{letter.toUpperCase()}</span>
    {/if}
  {/each}
</div>

<style>
  .container {
    display: flex;
    gap: 8px;
    font-size: 1.5rem;
    font-weight: bold;
    user-select: none;
  }

  .letter {
    &[data-state="typed"] {
      color: #77ff77;
    }

    &[data-state="current"] {
      color: var(--bg-color);
      background-color: var(--fg-color);
    }

    &[data-state="untyped"] {
      color: #505050;
    }
  }
</style>
