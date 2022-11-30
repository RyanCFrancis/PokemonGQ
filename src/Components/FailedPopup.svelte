<script lang="ts">
  import { fade, fly } from "svelte/transition";

  export let appear = false;
  export let name = "";

  function closeOut() {
    appear = false;
  }
</script>

{#if appear}
  <div class="contBG">
    <div class="cont" out:fade in:fade>
      <p>Nice Try! The pokemon's name was: {name}</p>
      <button
        class="closeButton"
        on:click={() => {
          closeOut();
        }}>close this popup</button
      >
    </div>
  </div>
{/if}

<style>
  .contBG {
    position: relative;
    top: 0%;
    left: 0%;
    width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.55);
    z-index: 2;
  }

  .cont {
    position: absolute;
    left: 50%;
    bottom: 50%;
    width: auto;
    max-width: 40vw;
    padding: 1%;
    overflow: auto;

    background-color: rgb(165, 81, 32);
    color: rgb(245, 245, 245);
    z-index: 3;
  }

  .closeButton {
  }
</style>
