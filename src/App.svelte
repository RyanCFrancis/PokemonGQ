<script lang="ts">
  import NamePKMN from "./Components/NamePKMN.svelte";
  import PokePic from "./Components/PokePic.svelte";
  import PKMNLoader from "./Components/PKMNLoader.svelte";
  import { onMount } from "svelte";

  let PKMNLoaderComp;

  let dexFound = false;

  let picLink: string;
  let isGuessed = false;
  //the name of the pokemon to guess
  let answer: string;
  let currentGuess: string;
  let quizCount = 0;
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

  async function getNext() {
    playerScore += currScore;
    let alertSTR = "You beat the game with a score of:" + playerScore;
    quizCount++;

    if (quizCount >= 5) {
      alert(alertSTR);
    }

    let tempNum = Math.floor(Math.random() * dexMax);
    //check if the same pokemon is picked and pick a new one
    if (tempNum == dexNum) {
      while (tempNum == dexNum) {
        tempNum = Math.floor(Math.random() * dexMax);
      }
    }
    dexNum = tempNum;
    PKMNLoaderComp.getPKMNData(dexNum);

    //reset some values

    isGuessed = false;
    guessCount = 10;
    currentGuess = "";
    currScore = 10;
  }

  onMount(async () => {
    dexNum = Math.floor(Math.random() * dexMax);
    PKMNLoaderComp.getPKMNData(dexNum);
  });

  function checkPKM(str: string) {
    //compare when lowercase to make it easier
    str = str.toLowerCase().trim();

    if (guessCount <= 0) {
      //code for next mon and popsup saying sorry nice try
      //correct answer was x
    }

    console.log("guess:", str);

    //const GUESSED_CORRECT: boolean = str.includes(answer.toLowerCase());
    const GUESSED_CORRECT: boolean = str === answer.toLocaleLowerCase();

    if (GUESSED_CORRECT) {
      //change css of the image to be revealed
      console.log("answer is guessed");
      isGuessed = true;
      //congrats popup goes here
    }

    if (!isGuessed) {
      guessCount--;
      currScore--;
    }
  }

  //global event if enter is pressed page wide
  function onEnter(event: KeyboardEvent) {
    if (event.key.localeCompare("Enter") === 0 && isGuessed) {
      getNext();
    }

    if (event.key.localeCompare("Enter") === 0) {
      checkPKM(currentGuess);
    }
  }
</script>

<main>
  <PKMNLoader
    bind:picLink
    bind:answer
    bind:dexFound
    bind:this={PKMNLoaderComp}
  />

  <div class="sidebar">
    <p>Score: {playerScore}</p>
    <p>Guesses Left: {guessCount}</p>
    <input
      placeholder="Enter PKMN Name Here"
      class="textbox"
      bind:value={currentGuess}
      on:keydown={onEnter}
    />
    <div style="width:100%; margin:2%;" />
    <button
      class="Button sub"
      on:click={() => {
        checkPKM(currentGuess);
      }}>Submit</button
    >
    <!-- hints go here -->
    <div style="width:100%; margin:2%;" />
    <button
      class="Button refresh"
      on:click={() => {
        getNext();
      }}>Next Pokemon</button
    >
  </div>
  <div class="rightSide">
    <PokePic isVis={isGuessed} srcLink={picLink} />
    {#key isGuessed}
      {#key answer}
        <NamePKMN pokeName={answer} isG={isGuessed} gCount={guessCount} />
      {/key}
    {/key}
  </div>
</main>

<style>
  /* .textbox {
  } */

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
    /* position: absolute;
    left: 2.5%;
    top: 50%; */
    width: auto;
  }
  .Button.refresh {
    /* position: absolute;
    left: 2.5%;
    top: 90%; */
    width: auto;
    justify-content: end;
  }

  .sidebar {
    position: fixed;
    top: 0;
    left: 0;
    height: 100%;
    display: inline-block;

    border-right: 1px solid rgb(0, 0, 0);
    background: rgb(161, 44, 44);
    overflow-y: auto;
    width: 25vw;
    min-width: 100px;
  }
</style>
