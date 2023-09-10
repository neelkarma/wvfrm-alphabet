<script lang="ts">
  import { createEventDispatcher } from "svelte";

  let el: HTMLInputElement;

  const dispatch = createEventDispatcher<{ input: string }>();

  export const focus = () => el.focus();
  export const blur = () => el.blur();

  const handleInput = () => {
    dispatch("input", el.value.toLowerCase());
    el.value = "";
  };
</script>

<input on:input={handleInput} bind:this={el} />

<style>
  input {
    border: 0;
    clip: rect(0 0 0 0);
    height: 1px;
    margin: -1px;
    overflow: hidden;
    padding: 0;
    position: absolute;
    width: 1px;
  }
</style>
