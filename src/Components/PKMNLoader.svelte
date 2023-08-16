<script lang="ts">
  export let picLink: string;
  export let answer: string;
  export let dexFound: boolean;

  export function getPKMNData(dexNum: number) {
    //link of the api as a string
    let linkSTR: string;
    linkSTR = "https://pokeapi.co/api/v2/pokemon/" + dexNum;
    fetch(linkSTR)
      .then((response) => response.json())
      .then((poke) => {
        //set the variables from the API
        picLink = poke.sprites.front_default;
        answer = poke.name;

        console.log("the pokemon is:", answer);
        //console.log(picLink);
        dexFound = true;
      });
  }
</script>

<div />

<style></style>
