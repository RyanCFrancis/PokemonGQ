<!-- <script lang="ts">
  import { PKMNLink, PKMNName } from "../store";

  //image link string from the api
  let picLink: string;
  //name of the pokemon to guess
  let answer: string;
  //true if the api has been reached successfully

  export async function getPKMNData(dexNum: number) {
    //link of the api as a string
    let linkSTR: string;
    linkSTR = "https://pokeapi.co/api/v2/pokemon/" + dexNum;
    fetch(linkSTR)
      .then((response) => response.json())
      .then((poke) => {
        //set the variables from the API
        picLink = poke.sprites.front_default;
        PKMNLink.set(picLink);
        answer = poke.name;
        PKMNName.set(answer);

        console.log("the pokemon is:", answer);
        //console.log(picLink);
      });
  }
</script>

<div />

<style></style> -->
