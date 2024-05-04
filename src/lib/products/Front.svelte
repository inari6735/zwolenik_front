<script>
  import { Button } from "flowbite-svelte";
 
  let wysokosc = 100;
  let szerokosc = 100;
  const grubosci = [18, 19, 22];
  let grubosc = grubosci[0];
  const kolory = ["Czerwony", "Zielony", "Niebieski", "Czarny"];
  let kolor = kolory[0];

  export let product = undefined;

  let price = 0;

  let error = undefined;


  function saveConfigurations(event) {
    event.preventDefault();

 
    
    if (wysokosc > 2750 || wysokosc < 50 ) {
      error = {
        msg: "Wysokość musi mieścić się w przedziale 50 > x > 2750"
      }
      return;
    }

    if (szerokosc > 2000 || szerokosc < 50 ) {
      error = {
        msg: "Szerokość musi mieścić się w przedziale 50 > x > 2000"
      }
      return;
    }

    if (grubosc !== 18 && grubosc !== 19 && grubosc !== 22 ) {
      error = {
        msg: "Zła grubość"
      }
      return;
    }



   
    const newConfiguration = {
      id: product.id,
      wysokosc: wysokosc,
      szerokosc: szerokosc,
      grubosc: grubosc,
      kolor: kolor,
      price: price,
      order: true,
    };
    
    try {
      const savedConfigurations = localStorage.getItem("specification");
      const configurations = savedConfigurations
        ? JSON.parse(savedConfigurations)
        : [];
      configurations.push(newConfiguration);
      localStorage.setItem("specification", JSON.stringify(configurations));
      console.log("Zapisano nową konfigurację:", newConfiguration);
    } catch (error) {
      console.error("Błąd podczas zapisu konfiguracji:", error);
    }
  }

  $: if(product.price){
    price = product.price;
  }

  function closeModal() {
        error = false;
    }
</script>

<div class="container">
  <div class="image-container">
    <img src="/SW32313_13242.png" alt="Obraz Produktu" class="product-image" />
  </div>

  {#if error}
  <div class="modal">
      <div class="update-form">
          <p>{error.msg}</p>
          <button class="btn submit" on:click={closeModal}>OK</button>
      </div>
  </div>
{/if}
  <div class="info-container">
    <h1>{product.name}</h1>

    <form on:submit|preventDefault={saveConfigurations} class="form-container">
      <label for="wysokosc">Wysokość [mm]:</label>
      <input
        type="number"
        id="wysokosc"
        bind:value={wysokosc}
        min="50"
        max="2750"
      />

      <h1 for="szerokosc">Szerokość [mm]:</h1>
      <input
        type="number"
        id="szerokosc"
        bind:value={szerokosc}
        min="50"
        max="2000"
      />

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

    <Button
      type="button"
      class=""
      style="background:#73AD21;"
      on:click={saveConfigurations}
    >
      Zapisz Konfigurację</Button
    >
  </div>
</div>

<style>
 .update-form{
        border: 6px solid #73ad21;
        border-radius: 10px;
    }

    .modal {
        
        display: flex;
        position: fixed;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        justify-content: center;
        align-items: center;
        background-color: rgba(0, 0, 0, 0.5); /* Semi-transparent background */
    }
    .modal > div {
        
        padding: 20px;
        background: white;
        border: 1px solid #ccc;
        z-index: 10;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    
    .btn {
        color: white;
        padding: 0.5rem 0;
        margin-top: 0.5rem;
        display: inline-block;
        width: 100%;
        border-radius: 0.25rem;
        cursor: pointer;
    }
    .submit {
        background: linear-gradient(to bottom, #50b01c 5%, #73ad21 100%);
        background-color: #50b01c;
    }
  .container {
    padding: 50px;
    margin: auto;
    border-radius: 25px;
    border: 6px solid #73ad21;
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
    border: 6px solid #73ad21;
    padding: 20px;
    border-radius: 25px;
    display: block;
  }
  label,
  input,
  select,
  button {
    display: block;
    margin-top: 10px;
  }

  @media (max-width: 640px) {
    .container {
      flex-direction: column;
    }
    .info-container,
    .image-container {
      width: 100%;
    }
    .form-container {
      width: auto;
    }
  }
</style>
