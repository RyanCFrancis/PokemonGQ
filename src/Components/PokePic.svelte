<script lang="ts">
  import PKMNLoader from "./PKMNLoader.svelte";
  export let isVis: boolean;
  let picLink: string;
  let answer: string;
  let dexFound: boolean;
  //import { getPKMNData } from "../App.svelte";
  isVis = false;

  export let srcLink: string;
  //TODO: ASYNC LOADING WHEN TRANSITIONING TO NEXT POKEMON JPEG

  $: activeClass = isVis ? "visible" : "hidden";
</script>

<PKMNLoader bind:picLink bind:answer bind:dexFound />

<div class="cont">
  <img
    src={srcLink}
    alt="Uh Oh! An Error has Occured! Try Refreshing!"
    class="pokepic {activeClass}"
    draggable="false"
  />
</div>

<style>
  .cont {
    position: relative;

    top: 50%;
    left: 50%;
  }
  .pokepic {
    width: 40vw;
    height: auto;
  }

  .pokepic.hidden {
    filter: brightness(0%) contrast(0%);
  }

  .pokepic.visible {
    filter: brightness(100%);
  }
</style>
