<script lang="ts">
  export let isVis: boolean;
  isVis = false;

  export let srcLink: string;

  $: activeClass = isVis ? "visible" : "hidden";
</script>

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
