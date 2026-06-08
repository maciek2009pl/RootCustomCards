<script>
    let { show, source  } = $props();
    import cardBase from '/src/lib/assets/cardBase.png';
</script>
<img hidden={!show} class="shadow/90" src={cardBase} alt="Card Base"/>
<img hidden={show} class="aspect-72/100 object-cover shadow/90" draggable=true src={source} alt="Card Base"/>
     
<style>

</style>