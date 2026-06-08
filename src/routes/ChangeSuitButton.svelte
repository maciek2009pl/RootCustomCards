<script lang="ts">
    let { disable, source, suit, addHandler, twColors } = $props();
</script>


<button disabled={disable} class="buttonBig {twColors} bg-gray-50 shadow-lg/30 border-white border-3 rounded-full" onclick={() => {addHandler(suit);}}>
    <img class="buttonBigInside object-contain" src={source} alt="Suit"/>
</button>


<style>
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
</style>