<script lang="ts">
  import "@fontsource/jetbrains-mono";
  import "tailwindcss/src/css/preflight.css";
  import "../styles.css";
  import type { Snippet } from "svelte";

  interface Props {
    children: Snippet;
  }

  let { children }: Props = $props();
</script>

<svelte:head>
  <title>Waveform Alphabet</title>
</svelte:head>

<div class="center">
  <div class="container">
    {@render children()}
  </div>
</div>

<style>
  .center {
    display: grid;
    height: 100vh;
    place-items: center;
  }

  .container {
    display: flex;
    padding: 8px;
    flex-direction: column;
    gap: 8px;
  }
</style>
