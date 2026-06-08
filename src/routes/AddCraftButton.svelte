<script>
    let { type, image, addHandler  } = $props();

</script>
<button class="buttonBig bg-yellow-600 hover:bg-yellow-800  shadow-lg/30 font-bold rounded" onclick={() => {addHandler(type);}}>
    <img class="buttonBigInside object-contain" src={image} alt="Symbol"/>
</button>
<style>
    .buttonBig{
    width: 4rem;
    height: 4rem;
    padding: 0.5rem;
  }
    .buttonBigInside{
    width: 3rem;
    height: 3rem;
  }
</style>