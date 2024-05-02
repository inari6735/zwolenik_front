<script>
    import { onMount } from 'svelte';
    import ProductsCart from "$lib/ProductsCart.svelte";
    
    let konfiguracje = [];
  
    onMount(() => {
      const zapisaneKonfiguracje = localStorage.getItem('konfiguracjeProduktow');
      if (zapisaneKonfiguracje) {
        konfiguracje = JSON.parse(zapisaneKonfiguracje);
        console.log('Odczytano konfiguracje:', konfiguracje);
      }
    });
  </script>
  
  <div>
    {#each konfiguracje as konfiguracja}
      <div>
        <ProductsCart/>
        <p>Id :{konfiguracja.id}</p>
        <p>Wysokość: {konfiguracja.wysokosc}</p>
        <p>Szerokość: {konfiguracja.szerokosc}</p>
        <p>Grubość: {konfiguracja.grubosc}</p>
        <p>Kolor: {konfiguracja.kolor}</p>
      </div>
    {/each}
  </div>
  
  