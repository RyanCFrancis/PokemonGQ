<script lang="ts">
  import NamePKMN from "./Components/NamePKMN.svelte";
  import PokePic from "./Components/PokePic.svelte";

  import { onMount } from "svelte";

  let dexFound = false;

  let picLink: string;
  let isGuessed = false;
  //the name of the pokemon to guess
  let answer: string;
  let currentGuess: string;
  let guessCount = 10;
  //pokedex number of the pokemon
  let dexNum: number;

  //players overall score for the quiz
  let playerScore = 0;
  //score for the current pokemon, starts at 10 and goes down with each incorrect answer.
  let currScore = 10;

  const dexMax = 300;

  currentGuess = "";

  //initilize some set of data on page load

  function getNext() {
    playerScore += currScore;
    let tempNum = Math.floor(Math.random() * dexMax);
    //check if the same pokemon is picked and pick a new one
    if (tempNum == dexNum) {
      while (tempNum == dexNum) {
        tempNum = Math.floor(Math.random() * dexMax);
      }
    }
    dexNum = tempNum;
    getPKMNData();

    //reset some values

    isGuessed = false;
    guessCount = 10;
    currentGuess = "";
    currScore = 10;
  }

  onMount(async () => {
    dexNum = Math.floor(Math.random() * dexMax);
    getPKMNData();
  });
  function getPKMNData() {
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

  function checkPKM(str: string) {
    //compare when lower case to make it easier
    str = str.toLowerCase();

    if (guessCount <= 0) {
      //code for next mon and popsup saying sorry nice try
      //correct answer was x
    }

    console.log("guess:", str);
    if (str.includes(answer.toLowerCase())) {
      //change css of the image to be revealed
      console.log("answer is guessed");
      isGuessed = true;
      //congratz popup goes here
    }
    if (!isGuessed) {
      guessCount--;
      currScore--;
    }
  }

  function onEnter(event: KeyboardEvent) {
    if (event.key.localeCompare("Enter") === 0) {
      checkPKM(currentGuess);
    }
  }
</script>

<main>
  <PokePic isVis={isGuessed} srcLink={picLink} />
  <div class="sidebar">
    <input
      placeholder="Enter PKMN Name Here"
      class="textbox"
      bind:value={currentGuess}
      on:keydown={onEnter}
    />
    <p>Score: {playerScore}</p>
    <p>Guesses Left: {guessCount}</p>

    <button
      class="Button sub"
      on:click={() => {
        checkPKM(currentGuess);
      }}>Submit</button
    >
    <button
      class="Button refresh"
      on:click={() => {
        getNext();
      }}>Next Pokemon</button
    >
  </div>

  <!-- really messy way to update -->
  {#key isGuessed}
    {#key answer}
      <NamePKMN pokeName={answer} isG={isGuessed} gCount={guessCount} />
    {/key}
  {/key}
</main>

<style>
  .textbox {
    position: absolute;
    left: 2.5%;
    top: 5%;
    width: auto;
  }

  .Button {
    background: #000000;

    border-radius: 15px;
    font-family: Georgia;
    color: #ffffff;
    font-size: 20px;
    padding: 10px 20px 10px 20px;
    text-decoration: none;
  }

  .Button.sub {
    position: absolute;
    left: 2.5%;
    top: 50%;
    width: auto;
  }
  .Button.refresh {
    position: absolute;
    left: 2.5%;
    top: 90%;
    width: auto;
  }

  .sidebar {
    position: fixed;
    top: 0;
    left: 0;
    height: 100%;

    border-right: 1px solid rgb(0, 0, 0);
    background: rgb(161, 44, 44);
    overflow-y: auto;
    width: 30vw;
    min-width: 100px;
  }
</style>
