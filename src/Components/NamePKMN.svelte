<script lang="ts">
  import { onMount } from "svelte";
  //full name of the pokemon
  export let pokeName: string;

  //array for if the letter is revealed in the hint or not
  //if true the letter is previously revealed
  //false if not
  let isHinted: Array<boolean> = [];

  //string to display to the user
  let hintStr = "";

  export async function getHintStr(guess: String) {
    hintStr = "";
    guess = guess?.toLocaleLowerCase().trim();
    //hintStr = currentHint;
    //var date = new Date();
    //console.log(date.getSeconds());
    //console.log("name in hintstr:", pokeName);

    //bug testing
    if (pokeName == undefined) {
      console.log("failed");
      return;
    } else if (guess === pokeName) {
      hintStr = pokeName;
      isHinted = [];
      return;
    }

    for (let i = 0; i < pokeName.length; i++) {
      let real_Letter = pokeName?.charAt(i);
      let guess_Letter = guess?.charAt(i);

      if (real_Letter === guess_Letter || isHinted[i]) {
        hintStr += real_Letter + " ";
        isHinted[i] = true;
      } else {
        hintStr += "_ ";
      }
    }
    hintStr.trimEnd();

    //currentHint = hintStr;
    //remove the extra space at the end
    //hintStr = hintStr.substring(0, hintStr.length - 1);

    //console.log("hint:", hintStr);
  }

  onMount(() => {
    getHintStr("");
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
