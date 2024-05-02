<script>
    import { Button } from 'flowbite-svelte';
  
    let wysokosc = 100;
    let szerokosc = 100;
    const grubosci = [18, 19, 22];
    let grubosc = grubosci[0];
    const kolory = ['Czerwony', 'Zielony', 'Niebieski', 'Czarny'];
    let kolor = kolory[0];
    
    export let product =undefined;

    function zapiszKonfiguracje(event) {
      event.preventDefault();
  
      // Przygotowanie obiektu z danymi konfiguracji
      const nowaKonfiguracja = {
        id: product.id,
        wysokosc: wysokosc,
        szerokosc: szerokosc,
        grubosc: grubosc,
        kolor: kolor
      };
  
      // Odczytanie aktualnej listy konfiguracji z localStorage
      const zapisaneKonfiguracje = localStorage.getItem('konfiguracjeProduktow');
      const konfiguracje = zapisaneKonfiguracje ? JSON.parse(zapisaneKonfiguracje) : [];
  
      // Dodanie nowej konfiguracji do listy
      konfiguracje.push(nowaKonfiguracja);
  
      // Zapisanie zaktualizowanej listy konfiguracji do localStorage
      localStorage.setItem('konfiguracjeProduktow', JSON.stringify(konfiguracje));
  
      console.log('Zapisano nową konfigurację:', nowaKonfiguracja);
    }
  </script>
  
  <div class="container">
    <div class="image-container">
      <img src="/SW32313_13242.png" alt="Obraz Produktu" class="product-image" />
    </div>
  
    <div class="info-container">
      <h1>{product.name}</h1>
  
      <form on:submit|preventDefault={zapiszKonfiguracje} class="form-container">
        <label for="wysokosc">Wysokość [mm]:</label>
        <input type="number" id="wysokosc" bind:value={wysokosc} min="50" max="2750" />

        <h1 for="szerokosc">Szerokość [mm]:</h1>
        <input type="number" id="szerokosc" bind:value={szerokosc} min="50" max="2000" />
  
        <label for="grubosc">Grubość [mm]:</label>
        <select id="grubosc" bind:value={grubosc}>
          {#each grubosci as g}
            <option value={g}>{g}</option>
          {/each}
        </select>
  
        <label for="kolor">Kolor:</label>
        <select id="kolor" bind:value={kolor}>
          {#each kolory as k}
            <option value={k}>{k}</option>
          {/each}
        </select>
      </form>
  
      <Button type="button" class="" style="background:#73AD21;" on:click={zapiszKonfiguracje}>
        Zapisz Konfigurację</Button>
    </div>
  </div>
  
  <style>
    .container {
      padding: 50px;
      margin: auto;
      border-radius: 25px;
      border: 6px solid #73AD21;
      display: flex;
      justify-content: center;
      align-items: start;
      gap: 20px;
      max-width: 1200px; /* Ograniczenie szerokości kontenera */
      width: 100%;
      box-sizing: border-box;
    }
    .image-container {
      flex: 1;
    }
    .product-image {
      width: 100%;
      height: auto;
    }
    .info-container {
      display: flex;
      flex-direction: column;
      flex: 2;
      gap: 20px;
    }
    .form-container {
      border: 6px solid #73AD21;
      padding: 20px;
      border-radius: 25px;
      display: block;
    }
    label, input, select, button {
      display: block;
      margin-top: 10px;
    }
  
    @media (max-width: 640px) {
      .container {
        flex-direction: column;
      }
      .info-container, .image-container {
        width: 100%;
      }
      .form-container {
        width: auto;
      }
    }
  </style>
  