<script>
    import Product from "$lib/Product.svelte";
    import Front from "$lib/Front.svelte";
    export let data;



</script>

<div class="justify-center">


    <Front product={data.data}/>

        <!-- <Front product={data.data}/> -->
 

</div>



