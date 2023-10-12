<script lang="ts">
  import { onMount } from "svelte";
  //full name of the pokemon
  export let pokeName: string;

  //string to display to the user
  let hintStr = "";

  export async function getHintStr(guess: String) {
    //hintStr = "";
    var date = new Date();
    console.log(date.getSeconds());
    console.log("name in hintstr:", pokeName);

    //bug testing
    if (pokeName == undefined) {
      console.log("failed");
      return;
    } else if (guess === pokeName) {
      hintStr = pokeName;
      return;
    } else {
      for (let i = 0; i < pokeName.length; i++) {
        hintStr += "_ ";
      }
    }

    //remove the extra space at the end
    //hintStr = hintStr.substring(0, hintStr.length - 1);

    console.log("hint:", hintStr);
  }

  // onMount(() => {
  //   getHintStr("");
  // });
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
