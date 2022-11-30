<script lang="ts">
  import { onMount } from "svelte";

  //full name of the pokemon
  export let pokeName: string;
  export let isG: boolean;
  export let gCount: number;
  //string to display to the user
  let hintStr = "";

  function getHintStr() {
    //console.log("name in hintstr:", pokeName);

    //bug testing
    if (pokeName === undefined) {
      //console.log("failed");
      return;
    }

    if (isG) {
      hintStr = pokeName;

      return;
    }
    for (let i = 0; i < pokeName.length; i++) {
      hintStr += "_ ";
    }

    //remove the extra space at the end
    //hintStr = hintStr.substring(0, hintStr.length - 1);

    console.log("hint:", hintStr);
  }

  onMount(() => {
    getHintStr();
  });
</script>

<div class="textDis">{hintStr}</div>

<style>
  .textDis {
    position: absolute;
    right: 20%;
    top: 85%;
    width: auto;
    font-size: 60px;
  }
</style>
