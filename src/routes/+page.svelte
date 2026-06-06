<script lang="ts">
  let craftSymbols = $state<CraftSymbol[]>([]);
  let name = '<strong>Svelte</strong>';
  let x = $state(0);
  let xtest = $state(0);
  let cardSuit = $state('');
  let activeButton  = $state('');
  let cardBox = $state('');
  let suitButtonState = $state([
    {suit: 'wild', isClicked: false},
    {suit: 'mouse', isClicked: false}, 
    {suit: 'rabbit', isClicked: false},  
    {suit: 'fox', isClicked: false}
  ]);
  let boxButtonState = $state([
    {suit: 'paper', isClicked: false},
    {suit: 'stone', isClicked: false}
  ]);
  import oneRabbit from '/src/lib/assets/onerabbit.png';
  import oneMouse from '/src/lib/assets/onemouse.png';
  import oneFox from '/src/lib/assets/onefox.png';
  import oneWild from '/src/lib/assets/onewild.png';
  import rabbitSuit from '/src/lib/assets/Icon - Rabbit.png';
  import mouseSuit from '/src/lib/assets/Icon - Mouse.png';
  import foxSuit from '/src/lib/assets/Icon - Fox.png';
  import wildSuit from '/src/lib/assets/Icon - Bird.png';
  import cardBase from '/src/lib/assets/card-swapmeet2.webp';
  import paperBox from '/src/lib/assets/paperboxMini.png';
  import stoneBoxMini from '/src/lib/assets/stoneboxMini.png';
  import stoneBox from '/src/lib/assets/stonebox2.png';
  import Nested from './nested.svelte';
  import emptySuit from '/src/lib/assets/empty icon.png';
  import { onMount } from 'svelte';

  onMount(() => {
    console.log("mount");
    craftSymbols = [new CraftSymbolEmpty(), new CraftSymbolEmpty(), new CraftSymbolEmpty(), new CraftSymbolEmpty(), new CraftSymbolEmpty(), new CraftSymbolEmpty()];
  });

  class CraftSymbol {
    sortNumber: number;
    urlName: string;
    public constructor(sortNumber: number, urlName: string) {
      this.sortNumber = sortNumber;
      this.urlName = urlName;
    }
  }
  class CraftSymbolRabbit extends CraftSymbol {
    public constructor() {
      super(3, oneRabbit);
    }
  }
  class CraftSymbolMouse extends CraftSymbol {
    public constructor() {
      super(1, oneMouse);
    }
  }
  class CraftSymbolFox extends CraftSymbol {
    public constructor() {
      super(2, oneFox);
    }
  }
  class CraftSymbolWild extends CraftSymbol {
    public constructor() {
      super(4, oneWild);
    }
  }
  class CraftSymbolEmpty extends CraftSymbol {
    public constructor() {
      super(0, emptySuit);
    }
  }
  function addCraftSymbol(type: string) {
    if(x<6) {
      switch(type) {
        case 'wild':
          craftSymbols[x] = new CraftSymbolWild();
          break;
        case 'rabbit':
          craftSymbols[x] = new CraftSymbolRabbit();
          break;
        case 'mouse':
          craftSymbols[x] = new CraftSymbolMouse();
          break;
        case 'fox':
          craftSymbols[x] = new CraftSymbolFox();
          break;
      }
      x++;
    }
    console.log(x);
    sortCraftSymbols();
  }
  function removeCraftSymbol(i: number) {
    console.log(x);
    if(craftSymbols[i] instanceof CraftSymbolEmpty) {
      return;
    }
    else {
      craftSymbols[i] = new CraftSymbolEmpty();
      x--;
      sortCraftSymbols();
    }
  }
  function sortCraftSymbols() {
    craftSymbols.sort((a,b) => b.sortNumber - a.sortNumber);
  }

  function setCardSuit(suit: string) {
    suitButtonState.forEach((button) => {
      button.isClicked = false;
    });
    switch(suit) {
      case 'wild':
        cardSuit = wildSuit;
        suitButtonState[0].isClicked = true;
        break;
      case 'rabbit':
        cardSuit = rabbitSuit;
        suitButtonState[1].isClicked = true;
        break;
      case 'mouse':
        cardSuit = mouseSuit;
        suitButtonState[2].isClicked = true;
        break;
      case 'fox':
        cardSuit = foxSuit;
        suitButtonState[3].isClicked = true;
        break;
    }
  }
  function setBoxType(type: string) {
      boxButtonState.forEach((button) => {
        button.isClicked = false;
      });
      switch(type) {
        case 'paper':
          cardBox = paperBox;
          boxButtonState[0].isClicked = true;
          break;
        case 'stone':
          cardBox = stoneBox;
          boxButtonState[1].isClicked = true;
          break;
      }
    }
</script>

<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>

<!--<div class="flex justify-center items-center space-x-4 mt-4">
  
  <div class="grid-cols-6 grid-rows-1 position-absolute top-4 right-4">
    <img class="position-relative" src={cardBase}/>
    {#each craftSymbols as i}
      <img class="image1" src={i}/>
    {/each}
  </div>
  {#if xtest===0}
    <p>X</p>
  {:else if xtest===1}
    <img class="board" src={craftSymbols[0]}/>
  {:else}
    <div class="board grid grid-cols-2 grid-rows-6 gap-4 position-absolute top-0 left-0">
      {#each craftSymbols as i}
        <img class="image1" src={i}/>
      {/each}
    </div>
  {/if}
  
</div>
-->
<div class=" flex flex-row flex-wrap gap-4 justify-center">
  <div class="w-full mx-16 justify-center card:w-[450px] flex">
    <div>
      <img class="bleedBorder shadow/90" src={cardBase} alt="Card Base"/>
    </div>
  </div>

  <div class="w-9/10 lg:w-[calc(100%-650px)] mx-6">
    <label for="cardName" class="block mt-2 textsizer font-medium text-gray-700">Card Name</label>
    <input type="text" id="cardName" name="cardName" maxlength="30" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm smga:text-3xl gmga:text-5xl">
    
    <label for="cardDescription" class="block mt-4 textsizer font-medium text-gray-700">Card Description</label>
    <textarea id="cardDescription" name="cardDescription" maxlength="250" rows="6" class="resize-none field-sizing mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 text-sm smga:text-3xl gmga:text-5xl"></textarea>
    
    <div class="columns-1 xl:columns-2 mt-4">
      <label for="crafting" class="block lg:col textsizer font-medium text-gray-700">Card Suit</label>
      <div class="gap-1 mt-2 justify-center lg:justify-start items-center flex">
        <button disabled={suitButtonState[0].isClicked} class="buttonBig enabled:bg-blue-900 enabled:hover:bg-blue-100 bg-gray-50 shadow-lg/30 border-white border-3 rounded-full" onclick={() => {setCardSuit("wild");}}>
          <img class="buttonBigInside object-contain " src={wildSuit} alt="Wild Suit"/>
        </button>
        <button disabled={suitButtonState[1].isClicked} class="buttonBigRabbit enabled:bg-yellow-900 enabled:hover:bg-yellow-100 bg-gray-50  shadow-lg/30 border-white border-3 rounded-full" onclick={() => {setCardSuit("rabbit");}}>
          <img class="buttonBigInside object-contain" src={rabbitSuit} alt="Rabbit Suit"/>
        </button>
        <button disabled={suitButtonState[3].isClicked} class="buttonBig enabled:bg-red-900 enabled:hover:bg-red-100 bg-gray-50  shadow-lg/30 border-white border-3 rounded-full" onclick={() => {setCardSuit("fox");}}>
          <img class="buttonBigInside object-contain" src={foxSuit} alt="Fox Suit"/>
        </button>
        <button disabled={suitButtonState[2].isClicked} class="buttonBig enabled:bg-orange-900 enabled:hover:bg-orange-100 bg-gray-50  shadow-lg/30 border-white border-3 rounded-full" onclick={() => {setCardSuit("mouse");}}>
          <img class="buttonBigInside object-contain" src={mouseSuit} alt="Mouse Suit"/>
        </button>
      </div>

      <label for="loadedImage" class="block mt-8 flex sm:justify-start textsizer font-medium text-gray-700">Card Type</label>
      <div class="gap-1 mt-2 justify-center lg:justify-start items-center flex">
        <button disabled={boxButtonState[0].isClicked} class="buttonCardType enabled:bg-yellow-600 enabled:hover:bg-yellow-800 bg-yellow-900 shadow-lg/30 rounded" onclick={() => {setBoxType("paper");}}>
          <img class="buttonCardTypeInside object-contain" src={paperBox} alt="Paper Box"/>
        </button>
        <button disabled={boxButtonState[1].isClicked} class="buttonCardType enabled:bg-yellow-600 enabled:hover:bg-yellow-800 bg-yellow-900 shadow-lg/30 rounded" onclick={() => {setBoxType("stone");}}>
          <img class="buttonCardTypeInside object-contain" src={stoneBoxMini} alt="Stone Box"/>
        </button>
      </div>
    </div>

    <div class="columns-1 xl:columns-2 mt-4">
      <label for="crafting" class="block mr-4 textsizer font-medium text-gray-700">Crafting Icons</label>
      <div class="gap-1 mt-2 justify-center lg:justify-start items-center flex">
        <button class="buttonBig bg-yellow-600 hover:bg-yellow-800  shadow-lg/30 font-bold rounded" onclick={() => {addCraftSymbol("wild");}}>
          <img class="buttonBigInside object-contain" src={oneWild} alt="Wild Symbol"/>
        </button>
        <button class="buttonBig bg-yellow-600 hover:bg-yellow-800  shadow-lg/30 font-bold rounded" onclick={() => {addCraftSymbol("rabbit");}}>
          <img class="buttonBigInside object-contain" src={oneRabbit} alt="Rabbit Symbol"/>
        </button>
        <button class="buttonBig bg-yellow-600 hover:bg-yellow-800  shadow-lg/30 font-bold rounded" onclick={() => {addCraftSymbol("fox");}}>
          <img class="buttonBigInside object-contain" src={oneFox} alt="Fox Symbol"/>
        </button>
        <button class="buttonBig bg-yellow-600 hover:bg-yellow-800  shadow-lg/30 font-bold rounded" onclick={() => {addCraftSymbol("mouse");}}>
          <img class="buttonBigInside object-contain" src={oneMouse} alt="Mouse Symbol"/>
        </button>
      </div>

      <label for="craftingCurrent" class="block mt-8 flex sm:justify-start textsizer font-medium text-gray-700">Current Crafting Requirements</label>
      <div class="gap-1 mt-2 justify-center lg:justify-start items-center flex">
        <button class="buttonSmall bg-yellow-500 hover:bg-red-600  shadow-lg/20 font-bold rounded" onclick={() => {removeCraftSymbol(0)}}>
          <img class="buttonSmallInside object-contain" src={craftSymbols[0]?.urlName} alt="Craft Symbol 0"/>
        </button>
        <button class="buttonSmall bg-yellow-500 hover:bg-red-600  shadow-lg/20 font-bold rounded" onclick={() => {removeCraftSymbol(1)}}>
          <img class="buttonSmallInside object-contain" src={craftSymbols[1]?.urlName} alt="Craft Symbol 1"/>
        </button>
        <button class="buttonSmall bg-yellow-500 hover:bg-red-600  shadow-lg/20 font-bold rounded" onclick={() => {removeCraftSymbol(2)}}>
          <img class="buttonSmallInside object-contain" src={craftSymbols[2]?.urlName} alt="Craft Symbol 2"/>
        </button>
        <button class="buttonSmall bg-yellow-500 hover:bg-red-600  shadow-lg/20 font-bold rounded" onclick={() => {removeCraftSymbol(3)}}>
          <img class="buttonSmallInside object-contain" src={craftSymbols[3]?.urlName} alt="Craft Symbol 3"/>
        </button>
        <button class="buttonSmall bg-yellow-500 hover:bg-red-600  shadow-lg/20 font-bold rounded" onclick={() => {removeCraftSymbol(4)}}>
          <img class="buttonSmallInside object-contain" src={craftSymbols[4]?.urlName} alt="Craft Symbol 4"/>
        </button>
        <button class="buttonSmall bg-yellow-500 hover:bg-red-600  shadow-lg/20 font-bold rounded" onclick={() => {removeCraftSymbol(5)}}>
          <img class="buttonSmallInside object-contain" src={craftSymbols[5]?.urlName} alt="Craft Symbol 5"/>
        </button>
      </div>
    </div>
      
    <label for="loadedImage" class="block mt-8 flex sm:justify-start textsizer font-medium text-gray-700">Load Image</label>
    <button class="size-y-8 size-x-64 bg-yellow-600 hover:bg-yellow-800 p-1 text-gray-800 shadow-lg/30 font-bold rounded" onclick={() => { x++}}>
      <img class="w-6 h-6 object-contain" src={oneMouse} alt="Loaderx"/>
    </button>
  </div>
</div>


<style>
  :global(body) {
    background-image: url("src/lib/assets/rough-textured-wall2.jpg");
    background-size: cover;
    background-repeat: repeat;
  }
  p {
    color: rgb(211, 91, 0);
    font-family: 'Comics Sans MS', cursive;
    font-size: 1.5em;
  }   
  .textsizer {
    font-size: max(1.25vw,1rem);
  }
  .buttonSmall{
    width: max(2vw,2rem);
    height: max(2vw,2rem);
    padding: max(0.25vw,0.25rem);
  }
  .buttonSmallInside{
    width: max(1.5vw,1.5rem);
    height: max(1.5vw,1.5rem);  
  }
  .buttonBig{
    width: max(3vw,3rem);
    height: max(3vw,3rem);
    padding: max(0.5vw,0.5rem);
  }
  .buttonBigRabbit{
    width: max(3vw,3rem);
    height: max(3vw,3rem);
    padding: max(0.4vw,0.4rem);
  }
  .buttonBigInside{
    width: max(2vw,2rem);
    height: max(2vw,2rem);
  }
  .buttonCardType{
    width: max(6vw,6rem);
    height: max(3vw,3rem);
    padding: max(0.5vw,0.5rem);
  }
  .buttonCardTypeInside{
    width: max(6vw,6rem);
    height: max(2vw,2rem);
  }
  .bleedBorder{
    outline-offset: -10px;
    outline: 1px dashed black;
  }
</style>