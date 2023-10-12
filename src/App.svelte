<script lang="ts">
  import NamePKMN from "./Components/NamePKMN.svelte";
  import PokePic from "./Components/PokePic.svelte";
  import { onMount } from "svelte";
  import { PKMNLink, PKMNName } from "./store";

  //let PKMNLoaderComponent;
  let NamePKMNComponent;

  //the name of the pokemon to guess
  let answer: string;
  let dexFound = false;
  let picLink: string;

  var isGuessed = false;

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
      quizCount = 0;
      playerScore = 0;
    }

    let tempNum = Math.floor(Math.random() * dexMax);
    //check if the same pokemon is picked and pick a new one
    if (tempNum == dexNum) {
      while (tempNum == dexNum) {
        tempNum = Math.floor(Math.random() * dexMax);
      }
    }
    dexNum = tempNum;

    //PKMNLoaderComponent.getPKMNData(dexNum);
    getPKMNData(dexNum);

    //reset some values

    isGuessed = false;
    guessCount = 10;
    currentGuess = "";
    currScore = 10;
  }
  async function getPKMNData(dexNum: number) {
    //link of the api as a string
    let linkSTR: string;
    linkSTR = "https://pokeapi.co/api/v2/pokemon/" + dexNum;
    const response = await fetch(linkSTR)
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
    return response;
  }

  onMount(async () => {
    dexNum = Math.floor(Math.random() * dexMax);
    await getPKMNData(dexNum);
    NamePKMNComponent.getHintStr();
    //PKMNLoaderComponent.getPKMNData(dexNum);
    //answer = PKMNLoaderComponent.answer;
    //picLink = PKMNLoaderComponent.picLink;
  });

  function checkPKM(str: string) {
    //compare when lowercase to make it easier
    str = str.toLowerCase().trim();

    if (guessCount <= 0) {
      //code for next mon and popsup saying sorry nice try
      //correct answer was x
    }

    console.log("you guessed:", str);

    //const GUESSED_CORRECT: boolean = str.includes(answer.toLowerCase());
    const GUESSED_CORRECT: boolean = str === answer.toLocaleLowerCase();

    if (GUESSED_CORRECT) {
      //change css of the image to be revealed
      //console.log("answer is guessed");
      isGuessed = true;
      console.log("isg", isGuessed);
      //congrats popup goes here
    }

    //update hint string
    NamePKMNComponent.getHintStr(str);

    if (!isGuessed) {
      guessCount--;
      currScore--;
    }
  }

  //global event if enter is pressed page wide
  function onEnter(event: KeyboardEvent) {
    if (event.key.localeCompare("Enter") === 0 && isGuessed) {
      getNext();
    } else if (event.key.localeCompare("Enter") === 0) {
      checkPKM(currentGuess);
    }
  }
</script>

<main>
  <!-- <PKMNLoader bind:this={PKMNLoaderComponent} /> -->

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
    {#key answer}
      <NamePKMN pokeName={answer} bind:this={NamePKMNComponent} />
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
