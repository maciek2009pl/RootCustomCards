<script>
    import rootLogo from '/src/lib/assets/logo.png';
    let {color} = $props();
</script>

<div class="top-0 w-full justify-center {color} shadow/30 flex" > 
  <img src={rootLogo} class="opacity-100 w-full sm:w-1/2 lg:w-1/4 object-top m-2 align-center" alt="logo"/>
</div>