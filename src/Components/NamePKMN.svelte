<script lang="ts" context="module">
  //full name of the pokemon
  export let pokeName: string;

  //string to display to the user
  let hintStr = "";
  console.log("huh");

  function getHintStr() {
    //console.log("name in hintstr:", pokeName);
    if (pokeName === undefined) {
      console.log("failed");
      return;
    }

    hintStr = "";
    for (let i = 0; i < pokeName.length; i++) {
      hintStr += "_ ";
    }
    //remove the extra space at the end
    hintStr = hintStr.substring(0, hintStr.length - 1);

    //console.log("hint:", hintStr);
  }

  // onMount(() => {

  //   pokeName = get(name);
  //   getHintStr();
  // });
</script>

<div class="textDis">++{hintStr}</div>

<style>
  .textDis {
    position: absolute;
    right: 25%;
    top: 85%;
    width: auto;
    font-size: 60px;
  }
</style>
