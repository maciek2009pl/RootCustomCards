
<script lang="ts">

  let craftSymbols = $state<CraftSymbol[]>([]);
  let craftCounter = $state(0);
  let cardSuit = $state('');
  let circleType= $state('');
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
  let imageShowcase = $derived(files?.length ? URL.createObjectURL(files[0]) : undefined);
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
  import emptySuit from '/src/lib/assets/empty icon.png';
  import html2canvas from 'html2canvas-pro';
  import { Cropper } from '@we-are-singular/svelte-chop-chop';

  import { onMount } from 'svelte';
  let loaded = $state(false);
  let cropper: any;
  let hiddenCapture = $state(true);
  let cardTitle = $state("")
  let cardDescription = $state(""); 
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
  function showShowcase() {
    showcaseVisible = true;
  }


  function downloadURI(uri: string, name: string) {
    var link = document.createElement("a");
    link.download = name;
    link.href = uri;
    link.click();
  }

  function screenshot() {
    console.log("screenshot");
    const el = document.querySelector("#captureCard") as HTMLElement | null;
    if (!el) return;
    console.log("screenshot2");

    html2canvas(el, {
      useCORS: true,
      scale: 4
    }).then((canvas: HTMLCanvasElement) => {
      downloadURI(
        canvas
          .toDataURL("image/jpeg")
          .replace("image/jpeg", "image/octet-stream"),
        "yourImage.jpg"
      );
    });
    console.log("screenshot3");
  }

</script>

{#snippet cropToolbar(cropper: any)}
  <button onclick={async () => {
    const result = await cropper.export({ format: 'image/webp', quality: 0.9 });
    if (result.blob) {
      const url = URL.createObjectURL(result.blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'cropped.webp';
      a.click();
    }
  }}>Crop</button>
{/snippet}

<div class=" flex flex-row flex-wrap mt-8 gap-4 justify-center">
  <div id="captureCard" class=" w-[250px] aspect-3/4 card:w-[450px] card:h-[625px] flex">
    <div class="relative">
      <img hidden={!showcaseVisible} class="shadow/90" src={cardBase} alt="Card Base"/>
      <img hidden={showcaseVisible} class="aspect-72/100 object-cover shadow/90" draggable=true src={imageShowcase} alt="Card Base"/>
      {#if cardBox!=""}
        <img class="absolute top-[256px] w-[172px] card:top-[464px] right-0 mr-4 mb-2 card:mr-6 card:mb-9 card:w-[310px]" src={cardBox} alt="Card Box"/>
        <img class="absolute left-0 bottom-0 ml-4 mb-2 card:ml-8 card:mb-11 w-[39px] card:w-[70px]" src={craftBG} alt="Card Box"/>
      {/if}

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
      <img class="absolute w-[19px] h-[19px] top-0 right-0 mr-5 mt-5 card:w-[36px] card:h-[36px] card:mr-9 card:mt-9" src={craftSymbols[0]?.urlName} alt={`Craft Symbol ${1}`}/>
      <img class="absolute w-[19px] h-[19px] top-0 right-0 mr-10 mt-5 card:w-[36px] card:h-[36px] card:mr-18 card:mt-9" src={craftSymbols[1]?.urlName} alt={`Craft Symbol ${2}`}/>
      <img class="absolute w-[19px] h-[19px] top-0 right-0 mr-15 mt-5 card:w-[36px] card:h-[36px] card:mr-27 card:mt-9" src={craftSymbols[2]?.urlName} alt={`Craft Symbol ${3}`}/>
      <img class="absolute w-[19px] h-[19px] top-0 right-0 mr-20 mt-5 card:w-[36px] card:h-[36px] card:mr-36 card:mt-9" src={craftSymbols[3]?.urlName} alt={`Craft Symbol ${4}`}/>
      <img class="absolute w-[19px] h-[19px] top-0 right-0 mr-25 mt-5 card:w-[36px] card:h-[36px] card:mr-45 card:mt-9" src={craftSymbols[4]?.urlName} alt={`Craft Symbol ${5}`}/>
      <img class="absolute w-[19px] h-[19px] top-0 right-0 mr-30 mt-5 card:w-[36px] card:h-[36px] card:mr-54 card:mt-9" src={craftSymbols[5]?.urlName} alt={`Craft Symbol ${6}`}/>

      <img class="absolute w-[19px] h-[19px] bottom-0 left-0 ml-5 mb-4 card:w-[36px] card:h-[36px] card:ml-10  card:mb-14" src={craftSymbols[0]?.urlName} alt={`Craft Symbol ${1}`}/>
      <img class="absolute w-[19px] h-[19px] bottom-0 left-0 ml-8 mb-5 card:w-[36px] card:h-[36px] card:ml-15 card:mb-16" src={craftSymbols[1]?.urlName} alt={`Craft Symbol ${2}`}/>
      <img class="absolute w-[19px] h-[19px] bottom-0 left-0 ml-5 mb-7 card:w-[36px] card:h-[36px] card:ml-10 card:mb-20" src={craftSymbols[2]?.urlName} alt={`Craft Symbol ${3}`}/>
      <img class="absolute w-[19px] h-[19px] bottom-0 left-0 ml-8 mb-8 card:w-[36px] card:h-[36px] card:ml-15 card:mb-22" src={craftSymbols[3]?.urlName} alt={`Craft Symbol ${4}`}/>
      <img class="absolute w-[19px] h-[19px] bottom-0 left-0 ml-5 mb-10 card:w-[36px] card:h-[36px] card:ml-10 card:mb-26" src={craftSymbols[4]?.urlName} alt={`Craft Symbol ${5}`}/>
      <img class="absolute w-[19px] h-[19px] bottom-0 left-0 ml-8 mb-11 card:w-[36px] card:h-[36px] card:ml-15 card:mb-28" src={craftSymbols[5]?.urlName} alt={`Craft Symbol ${6}`}/>
      <div class="absolute top-[258px] left-[70px] w-[154px] h-[20px] card:top-[470px] card:left-[135px] card:w-[280px] card:h-[40px] justify-center flex">
        <text class="font-[Luminari] text-[0.695em] card:text-[1.25em] relative whitespace-pre truncate  inset-x-0 break-all">{cardTitle}</text>
      </div>
      <div class="absolute top-[273px] left-[70px] w-[154px] h-[44px] card:top-[495px] card:left-[135px] card:w-[280px] card:h-[80px] justify-center flex">
        <text class="font-[Baskervville] text-[0.611em] card:text-[1.1em]  leading-[1.1em] whitespace-pre-wrap truncate items-center justify-center relative inset-x-0">{cardDescription}</text>
      </div>
    </div> 
  </div>
      

  <div class="w-9/10 lg:w-[calc(100%-650px)] mx-6">
    <label for="cardName" class="block mt-2 textsizer font-medium text-gray-700">Card Name</label>
    <input type="text" bind:value={cardTitle} id="cardName" name="cardName" maxlength="28" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 text-m smga:text-3xl">
    
    <label for="cardDescription" class="block mt-4 textsizer font-medium text-gray-700">Card Description</label>
    <textarea id="cardDescription" bind:value={cardDescription} name="cardDescription" maxlength="160" rows="4" class="resize-none field-sizing mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 text-m smga:text-3xl"></textarea>
    
    <!--
    <div class="columns-1 xl:columns-3 mt-4">
      <label for="cardName" class="block textsizer font-medium text-gray-700">Position from left (x)</label>
      <input type="number" id="cardName" bind:value={leftpos} name="cardName" maxlength="5" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 text-m smga:text-3xl">
      <label for="cardName" class="block mt-2 textsizer font-medium text-gray-700">Position from top (y)</label>
      <input type="number" id="cardName" bind:value={toppos} name="cardName" maxlength="5" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 text-m smga:text-3xl">
      <label for="cardName" class="block mt-2 textsizer font-medium text-gray-700">Scale (%)</label>
      <input type="number" id="cardName" bind:value={scalepos} name="cardName" maxlength="5" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 text-m smga:text-3xl">
    </div>
   -->

    <div class="columns-1 xl:columns-2 mt-4">
      <label for="crafting" class="block lg:col textsizer font-medium text-gray-700">Card Suit</label>
      <div class="gap-1 mt-2 justify-center lg:justify-start items-center flex">
        <button disabled={suitButtonState[0].isClicked} class="buttonBig enabled:bg-blue-900 enabled:hover:bg-blue-100 bg-gray-50 shadow-lg/30 border-white border-3 rounded-full" onclick={() => {setCardSuit("wild");}}>
          <img class="buttonBigInside object-contain" src={wildSuit} alt="Wild Suit"/>
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

    <div class="columns-1 2xl:columns-2 mt-4">
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
      {#each craftSymbols as symbol, i}
        <button class="buttonSmall bg-yellow-500 hover:bg-red-600  shadow-lg/20 font-bold rounded" onclick={() => {removeCraftSymbol(i)}}>
          <img class="buttonSmallInside object-contain" src={symbol?.urlName} alt={`Craft Symbol ${i}`}/>
        </button>
      {/each}
      </div>
    
    </div>
      
    <label for="loadxdddd" class="block mt-8 flex sm:justify-start textsizer font-medium text-gray-700">Load Image</label>
    <button command="show-modal" commandfor="dialog" class="bg-yellow-600 hover:bg-yellow-800 p-3 shadow-lg/30 font-bold rounded" onclick={() => {showShowcase()} }>Open dialog</button>
    <el-dialog>
      <dialog id="dialog" aria-labelledby="dialog-title" class="fixed inset-0 size-auto max-h-none max-w-none overflow-y-auto bg-transparent backdrop:bg-transparent">
        <el-dialog-backdrop class="fixed inset-0 bg-gray-500/75 transition-opacity data-closed:opacity-0 data-enter:duration-300 data-enter:ease-out data-leave:duration-200 data-leave:ease-in"></el-dialog-backdrop>
        <div class="flex min-h-full items-end justify-center p-4 text-center focus:outline-none sm:items-center sm:p-0">

          <el-dialog-panel class="relative transform overflow-hidden rounded-lg bg-white text-left shadow-xl transition-all data-closed:translate-y-4 data-closed:opacity-0 data-enter:duration-300 data-enter:ease-out data-leave:duration-200 data-leave:ease-in sm:my-8 sm:w-full sm:max-w-lg data-closed:sm:translate-y-0 data-closed:sm:scale-95">
            <div class="w-full">
              <Cropper bind:this={cropper} src={imageShowcase} aspectRatio={72/100} style="height: 200px"/>
              <button type="button" command="close" commandfor="dialog" class="justify-center rounded-md bg-green-300 px-3 py-2 text-sm font-semibold text-gray-900 shadow-xs inset-ring inset-ring-gray-300 hover:bg-gray-50" onclick={() => { cropToolbar(cropper); hideShowcase(); }}>Crop</button>
            </div>
            <div>        
              
              <div class="bg-gray-50 m-4 flex">
                <!--<button type="button" aria-label="Upload image" class="w-1/2 m-2 justify-center rounded-md bg-red-600 px-3 py-2 text-sm font-semibold text-white shadow-xs hover:bg-red-500">
                -->  
                <input type="file" bind:files accept="image/png image/jpeg" id="loadedImage" name="avatar" class="w-1/2 m-2 justify-center rounded-md bg-red-600 px-3 py-2 text-sm font-semibold text-white shadow-xs hover:bg-red-500" onchange={() => { loaded = true}}/>
                <button type="button" command="close" commandfor="dialog" class="w-1/2 m-2 justify-center rounded-md bg-white px-3 py-2 text-sm font-semibold text-gray-900 shadow-xs inset-ring inset-ring-gray-300 hover:bg-gray-50">Cancel</button>
                <button type="button" command="close" commandfor="dialog" class="w-1/2 m-2 justify-center rounded-md bg-green-300 px-3 py-2 text-sm font-semibold text-gray-900 shadow-xs inset-ring inset-ring-gray-300 hover:bg-gray-50" onclick={() => { hideShowcase()  }}>Confirm</button>
              </div>
            </div>
          </el-dialog-panel>
        </div>
      </dialog>
    </el-dialog>
    <label for="loadxdd222" class="block mt-8 flex sm:justify-start textsizer font-medium text-gray-700">Merge Image</label>
    <button class="bg-yellow-600 hover:bg-yellow-800 p-3 shadow-lg/30 font-bold rounded" onclick={() => { screenshot() }}>Merge Image</button>
  </div>
</div>


<style>
  :global(body) {
    background-color: rgb(247, 235, 221);
    background-image: url("src/lib/assets/rough-textured-wall2.jpg");
    background-size: cover;
    background-repeat: repeat;
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
    font-size: 1rem;
  }
  .buttonSmall{
    width: 3rem;
    height: 3rem;
    padding: 0.5rem;
  }
  .buttonSmallInside{
    width: 2rem;
    height: 2rem;
  }
  .buttonBig{
    width: 4rem;
    height: 4rem;
    padding: 0.5rem;
  }
  .buttonBigRabbit{
    width: 4rem;
    height: 4rem;
    padding: 0.4rem;
  }
  .buttonBigInside{
    width: 3rem;
    height: 3rem;
  }
  .buttonCardType{
    width: 8rem;
    height: 4rem;
    padding: 0.5rem;
  }
  .buttonCardTypeInside{
    width: 8rem;
    height: 3rem;
  }
</style>