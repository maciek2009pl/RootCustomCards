<script lang="ts">
  let craftSymbols = $state<string[]>([]);
  let name = '<strong>Svelte</strong>';
  let x = $state(0);
  let xtest = $state(0);
  import oneRabbit from '/src/lib/assets/onerabbit.png';
  import oneMouse from '/src/lib/assets/onemouse.png';
  import oneFox from '/src/lib/assets/onefox.png';
  import oneWild from '/src/lib/assets/onewild.png';
  import rabbitSuit from '/src/lib/assets/Icon - Rabbit.png';
  import mouseSuit from '/src/lib/assets/Icon - Mouse.png';
  import foxSuit from '/src/lib/assets/Icon - Fox.png';
  import wildSuit from '/src/lib/assets/Icon - Bird.png';
  import cardBase from '/src/lib/assets/card-swapmeet2.webp';
  import paperBox from '/src/lib/assets/paperbox2.png';
  import stoneBox from '/src/lib/assets/stonebox2.png';
  import Nested from './nested.svelte';

  function addCraftSymbolRabbit() {
    if(xtest < 6)
    {
      craftSymbols.push(oneRabbit);
      xtest++;
      craftSymbols[xtest];
    }
  }
  function addCraftSymbolMouse() {
    if(xtest < 6)
    {
      craftSymbols.push(oneMouse);
      xtest++;
      craftSymbols[xtest];
    }
  }
  function addCraftSymbolFox() {
    if(xtest < 6)
    {
      craftSymbols.push(oneFox);
      xtest++;
      craftSymbols[xtest];
    }
  }
  function addCraftSymbolWild() {
    if(xtest < 6)
    {
      craftSymbols.push(oneWild);
      xtest++;
      craftSymbols[xtest];
    }
  }
</script>


<h1>Welcome to {@html name.toUpperCase()}</h1>
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
      <img class="bleedBorder shadow/90" src={cardBase}/>
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
        <button class="buttonBig bg-blue-900 hover:bg-blue-100 shadow-lg/30 border-white border-3 rounded-full" onclick={() => {addCraftSymbolWild(); x++}}>
          <img class="buttonBigInside object-contain" src={wildSuit}/>
        </button>
        <button class="buttonBig bg-yellow-900 hover:bg-yellow-100 shadow-lg/30 border-white border-3 rounded-full" onclick={() => {addCraftSymbolRabbit(); x++}}>
          <img class="buttonBigInside object-contain" src={rabbitSuit}/>
        </button>
        <button class="buttonBig bg-red-900 hover:bg-red-100 shadow-lg/30 border-white border-3 rounded-full" onclick={() => {addCraftSymbolFox(); x++}}>
          <img class="buttonBigInside object-contain" src={foxSuit}/>
        </button>
        <button class="buttonBig bg-orange-900 hover:bg-orange-100 shadow-lg/30 border-white border-3 rounded-full" onclick={() => {addCraftSymbolMouse(); x++}}>
          <img class="buttonBigInside object-contain" src={mouseSuit}/>
        </button>
      </div>

      <label for="loadedImage" class="block mt-8 flex sm:justify-start textsizer font-medium text-gray-700">Card Type</label>
      <div class="gap-1 mt-2 justify-center lg:justify-start items-center flex">
        <button class="buttonCardType bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolWild(); x++}}>
          <img class="buttonCardTypeInside object-contain" src={paperBox}/>
        </button>
        <button class="buttonCardType bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolWild(); x++}}>
          <img class="buttonCardTypeInside object-contain" src={stoneBox}/>
        </button>
      </div>
    </div>

    <div class="columns-1 xl:columns-2 mt-4">
      <label for="crafting" class="block mr-4 textsizer font-medium text-gray-700">Crafting Icons</label>
      <div class="gap-1 mt-2 justify-center lg:justify-start items-center flex">
        <button class="buttonBig bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolWild(); x++}}>
          <img class="buttonBigInside object-contain" src={oneWild}/>
        </button>
        <button class="buttonBig bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolRabbit(); x++}}>
          <img class="buttonBigInside object-contain" src={oneRabbit}/>
        </button>
        <button class="buttonBig bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolFox(); x++}}>
          <img class="buttonBigInside object-contain" src={oneFox}/>
        </button>
        <button class="buttonBig bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolMouse(); x++}}>
          <img class="buttonBigInside object-contain" src={oneMouse}/>
        </button>
      </div>

      <label for="craftingCurrent" class="block mt-8 flex sm:justify-start textsizer font-medium text-gray-700">Current Crafting Requirements</label>
      <div class="gap-1 mt-2 justify-center lg:justify-start items-center flex">
        <button class="buttonSmall bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolMouse(); x++}}>
          <img class="buttonSmallInside object-contain"  src={oneMouse}/>
        </button>
        <button class="buttonSmall  bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolMouse(); x++}}>
          <img class="buttonSmallInside object-contain" src={oneMouse}/>
        </button>
        <button class="buttonSmall bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolMouse(); x++}}>
          <img class="buttonSmallInside object-contain" src={oneMouse}/>
        </button>
        <button class="buttonSmall bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolMouse(); x++}}>
          <img class="buttonSmallInside object-contain" src={oneMouse}/>
        </button>
        <button class="buttonSmall bg-gray-300 hover:bg-gray-400 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolMouse(); x++}}>
          <img class="buttonSmallInside object-contain" src={oneMouse}/>
        </button>
      </div>
    </div>
      
    <label for="loadedImage" class="block mt-8 flex sm:justify-start textsizer font-medium text-gray-700">Load Image</label>
    <button class="size-y-8 size-x-64 bg-gray-300 hover:bg-gray-400 p-1 text-gray-800 font-bold rounded" onclick={() => {addCraftSymbolMouse(); x++}}>
      <img class="w-6 h-6 object-contain" src={oneMouse}/>
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