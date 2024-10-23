<script lang="ts">
  interface Props {
    oninput: (char: string) => void;
  }

  let { oninput }: Props = $props();

  let el: HTMLInputElement | undefined = $state();

  export const focus = () => el?.focus();
  export const blur = () => el?.blur();

  const handleInput = () => {
    if (!el) return;
    oninput(el.value.toLowerCase());
    el.value = "";
  };
</script>

<input oninput={handleInput} bind:this={el} />

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
