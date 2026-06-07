<script lang="ts">
  let craftSymbols = $state<CraftSymbol[]>([]);
  let craftCounter = $state(0);
  let cardSuit = $state('');
  let circleType= $state('');
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
  let files = $state<FileList | null | undefined>(null);
  let showcaseVisible = $state(true);
  let scalepos = $state(2);
  let leftpos = $state(0);
  let toppos = $state(0);

  import oneRabbit from '/src/lib/assets/onerabbit.png';
  import oneMouse from '/src/lib/assets/onemouse.png';
  import oneFox from '/src/lib/assets/onefox.png';
  import oneWild from '/src/lib/assets/onewild.png';
  import circleFox from '/src/lib/assets/Circle - Fox.png';
  import circleMouse from '/src/lib/assets/Circle - Mouse.png';
  import circleRabbit from '/src/lib/assets/Circle - Rabbit.png';
  import circleWild from '/src/lib/assets/Circle - Wild.png';
  import rabbitSuit from '/src/lib/assets/Icon - Rabbit.png';
  import mouseSuit from '/src/lib/assets/Icon - Mouse.png';
  import foxSuit from '/src/lib/assets/Icon - Fox.png';
  import wildSuit from '/src/lib/assets/Icon - Bird.png';
  import cardBase from '/src/lib/assets/cardBase.png';
  import paperBox from '/src/lib/assets/paperbox2.png';
  import paperBoxMini from '/src/lib/assets/paperboxMini.png';
  import stoneBoxMini from '/src/lib/assets/stoneboxMini.png';
  import stoneBox from '/src/lib/assets/stonebox2.png';
  import craftBG from '/src/lib/assets/craftBG.png';
  import Nested from './nested.svelte';
  import emptySuit from '/src/lib/assets/empty icon.png';
  import { onMount } from 'svelte';

  let loaded = $state(false);


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
    if(craftCounter<6) {
      switch(type) {
        case 'wild':
          craftSymbols[craftCounter] = new CraftSymbolWild();
          break;
        case 'rabbit':
          craftSymbols[craftCounter] = new CraftSymbolRabbit();
          break;
        case 'mouse':
          craftSymbols[craftCounter] = new CraftSymbolMouse();
          break;
        case 'fox':
          craftSymbols[craftCounter] = new CraftSymbolFox();
          break;
      }
      craftCounter++;
    }
    console.log(craftCounter);
    sortCraftSymbols();
  }
  function removeCraftSymbol(i: number) {
    console.log(craftCounter);
    if(craftSymbols[i] instanceof CraftSymbolEmpty) {
      return;
    }
    else {
      craftSymbols[i] = new CraftSymbolEmpty();
      craftCounter--;
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
        circleType = circleWild;
        suitButtonState[0].isClicked = true;
        break;
      case 'rabbit':
        cardSuit = rabbitSuit;
        circleType = circleRabbit;
        suitButtonState[1].isClicked = true;
        break;
      case 'mouse':
        cardSuit = mouseSuit;
        circleType = circleMouse;
        suitButtonState[2].isClicked = true;
        break;
      case 'fox':
        cardSuit = foxSuit;
        circleType = circleFox;
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

    function hideShowcase() {
      showcaseVisible = false;
    }
    function changePos() {
      return `top-[${toppos}px] left-[${leftpos}px] scale-[${scalepos}%]` ;
    }

</script>


<div class=" flex flex-row flex-wrap gap-4 justify-center">
  <div class=" w-[250px] aspect-3/4 mx-16 card:w-[450px] card:h-[625px] flex">
    <div class="relative">
      <img hidden={!showcaseVisible} class="bleedBorder shadow/90" src={cardBase} alt="Card Base"/>
      <img hidden={showcaseVisible} class="aspect-72/100 object-fit shadow/90" src={files?.length ? URL.createObjectURL(files[0]) : undefined} alt="Card Base"/>
      {#if cardBox!=""}
        <img class="absolute bottom-0 right-0 mr-4 mb-2 card:mr-6 card:mb-9 w-[172px] card:w-[310px]" src={cardBox} alt="Card Box"/>
      {/if}
      <img class="absolute left-0 bottom-0 ml-4 mb-2 card:ml-8 card:mb-11 w-[39px] card:w-[70px]" src={craftBG} alt="Card Box"/>
      {#if circleType==circleWild}
        <div class="absolute top-0 bg-birdcolor w-[250px] h-[44px] card:w-[450px] card:h-[85px]"></div>
        <div class="absolute top-0 mt-11 card:mt-21 bg-gray-50 w-[250px] h-[3px] card:w-[450px] card:h-[5px]"></div>
      {/if}
      {#if circleType==circleRabbit}
        <div class="absolute top-0 bg-rabbitcolor w-[250px] h-[44px] card:w-[450px] card:h-[85px]"></div>
        <div class="absolute top-0 mt-11 card:mt-21 bg-gray-50 w-[250px] h-[3px] card:w-[450px] card:h-[5px]"></div>
      {/if}
      {#if circleType==circleFox}
        <div class="absolute top-0 bg-foxcolor w-[250px] h-[44px] card:w-[450px] card:h-[85px]"></div>
        <div class="absolute top-0 mt-11 card:mt-21 bg-gray-50 w-[250px] h-[3px] card:w-[450px] card:h-[5px]"></div>
      {/if}
      {#if circleType==circleMouse}
        <div class="absolute top-0 bg-mousecolor w-[250px] h-[44px] card:w-[450px] card:h-[85px]"></div>
        <div class="absolute top-0 mt-11 card:mt-21 bg-gray-50 w-[250px] h-[3px] card:w-[450px] card:h-[5px]"></div>
      {/if}
      {#if circleType!=""}
        <img class="absolute left-0 top-0 ml-5 mt-5 card:ml-10 card:mt-10 w-[44px] h-[44px] card:w-[80px] card:h-[80px]" src={circleType} alt="Card Box"/>
      {/if}
      
    </div> 
  </div>
      

  <div class="w-9/10 lg:w-[calc(100%-650px)] mx-6">
    <label for="cardName" class="block mt-2 textsizer font-medium text-gray-700">Card Name</label>
    <input type="text" id="cardName" name="cardName" maxlength="30" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm smga:text-3xl gmga:text-5xl">
    
    <label for="cardDescription" class="block mt-4 textsizer font-medium text-gray-700">Card Description</label>
    <textarea id="cardDescription" name="cardDescription" maxlength="250" rows="6" class="resize-none field-sizing mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 text-sm smga:text-3xl gmga:text-5xl"></textarea>
    
    
    <div class="columns-1 xl:columns-3 mt-4">
      <label for="cardName" class="block textsizer font-medium text-gray-700">Position from left (x)</label>
      <input type="number" id="cardName" bind:value={leftpos} name="cardName" maxlength="5" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm smga:text-3xl gmga:text-5xl">
      <label for="cardName" class="block mt-2 textsizer font-medium text-gray-700">Position from top (y)</label>
      <input type="number" id="cardName" bind:value={toppos} name="cardName" maxlength="5" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm smga:text-3xl gmga:text-5xl">
      <label for="cardName" class="block mt-2 textsizer font-medium text-gray-700">Scale (%)</label>
      <input type="number" id="cardName" bind:value={scalepos} name="cardName" maxlength="5" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm smga:text-3xl gmga:text-5xl">
    </div>
   

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

      <label for="bruh" class="block mt-8 flex sm:justify-start textsizer font-medium text-gray-700">Card Type</label>
      <div class="gap-1 mt-2 justify-center lg:justify-start items-center flex">
        <button disabled={boxButtonState[0].isClicked} class="buttonCardType enabled:bg-yellow-600 enabled:hover:bg-yellow-800 bg-yellow-900 shadow-lg/30 rounded" onclick={() => {setBoxType("paper");}}>
          <img class="buttonCardTypeInside object-contain" src={paperBoxMini} alt="Paper Box"/>
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
    <button command="show-modal" commandfor="dialog" class="bg-yellow-600 hover:bg-yellow-800 p-3 shadow-lg/30 font-bold rounded">Open dialog</button>
    <el-dialog>
      <dialog id="dialog" aria-labelledby="dialog-title" class="fixed inset-0 size-auto max-h-none max-w-none overflow-y-auto bg-transparent backdrop:bg-transparent">
        <el-dialog-backdrop class="fixed inset-0 bg-gray-500/75 transition-opacity data-closed:opacity-0 data-enter:duration-300 data-enter:ease-out data-leave:duration-200 data-leave:ease-in"></el-dialog-backdrop>
        <div class="flex min-h-full items-end justify-center p-4 text-center focus:outline-none sm:items-center sm:p-0">
          <el-dialog-panel class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all data-closed:translate-y-4 data-closed:opacity-0 data-enter:duration-300 data-enter:ease-out data-leave:duration-200 data-leave:ease-in sm:my-8 sm:w-full sm:max-w-lg data-closed:sm:translate-y-0 data-closed:sm:scale-95">
            <div>
              <div hidden={loaded}>
                <p>Upload image and crop it.</p>
              </div>
              <div class="bg-gray-50 px-4 py-3 sm:flex sm:flex-row-reverse sm:px-6 flex">
                <button type="button" aria-label="Upload image" class="inline-flex w-full justify-center rounded-md bg-red-600 px-3 py-2 text-sm font-semibold text-white shadow-xs hover:bg-red-500 sm:ml-3 sm:w-auto">
                  <input type="file" bind:files accept="image/png image/jpeg" id="loadedImage" name="avatar" onchange={() => { loaded = true}}/>
                </button>
                <button type="button" command="close" commandfor="dialog" class="mt-3 mr-3 inline-flex w-full justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-xs inset-ring inset-ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto">Cancel</button>
                <button type="button" command="close" commandfor="dialog" class="mt-3 mr-3 inline-flex w-full justify-center rounded-md bg-green-300 px-3 py-2 text-sm font-semibold text-gray-900 shadow-xs inset-ring inset-ring-gray-300 hover:bg-gray-50 sm:mt-0 sm:w-auto" onclick={() => { hideShowcase() }}>Confirm Crop</button>
              </div>
            </div>
          </el-dialog-panel>
        </div>
      </dialog>
    </el-dialog>
    <label for="loadedImage" class="block mt-8 flex sm:justify-start textsizer font-medium text-gray-700">Merge Image</label>
    <button class="bg-yellow-600 hover:bg-yellow-800 p-3 shadow-lg/30 font-bold rounded">Merge Image</button>
  </div>
</div>


<style>
  :global(body) {
    background-color: rgb(247, 235, 221);
    background-image: url("src/lib/assets/rough-textured-wall2.jpg");
    background-size: cover;
    background-repeat: repeat;
  }
  p {
    color: rgb(211, 91, 0);
    font-family: 'Comics Sans MS', cursive;
    font-size: 1.5em;
  }  
  .bg-birdcolor{
    background-color: rgb(106, 190, 194);
  } 
  .bg-rabbitcolor{
    background-color: rgb(255, 231, 101);
  } 
  .bg-foxcolor{
    background-color: rgb(221, 79, 56);
  } 
  .bg-mousecolor{
    background-color: rgb(251, 157, 105);
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