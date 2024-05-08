<script>
  import { Button } from "flowbite-svelte";
  import { createEventDispatcher } from "svelte";

  const dispatch = createEventDispatcher();

  export let product = undefined;

  const toConfigure ={
    heightMin: 50,
    heightMax: 2750,
    widthMin: 50,
    widthMax: 2000,
    thickness: [18, 19, 22],
    color: ["Czerwony", "Zielony", "Niebieski", "Czarny"],
    price: 1,
  }

  const details= {
    height: toConfigure.heightMin,
    width: toConfigure.widthMin,
    thickness: toConfigure.thickness[0],
    color: toConfigure.color[0],
  };


  let price = 0;
  let amount = 1;
  details.price = product.price;



  // let wysokosc = 100;
  // let szerokosc = 100;
  // const grubosci = [18, 19, 22];
  // let grubosc = grubosci[0];
  // const kolory = ["Czerwony", "Zielony", "Niebieski", "Czarny"];
  // let kolor = kolory[0];

  let error = undefined;

  function decreaseAmount() {
    if (amount > 1) {
      amount -= 1;
      dispatch("change", { amount });
    }
  }

  function increaseAmount() {
    if (amount < 999) {
      amount += 1;
      dispatch("change", { amount });
    }
  }

  function handleInput(event) {
    amount = event.target.value;
    if (isNaN(amount)) {
      amount = 0;
    }
    if (amount > 999) {
      amount = 999;
    }
    if (amount < 1) {
      amount = 1;
      dispatch("change", { amount });
    }

    dispatch("change", { amount });
  }

  function saveConfigurations(event) {
    event.preventDefault();

    if (details.height > toConfigure.heightMax || details.height < toConfigure.heightMin) {
      error = {
        msg: "Wysokość musi mieścić się w przedziale " + toConfigure.heightMax + " > x > " + toConfigure.heightMin,
      };
      return;
    }

    if (details.width > toConfigure.widthMax || details.width < toConfigure.widthMin) {
      error = {
        msg: "Szerokość musi mieścić się w przedziale "+ toConfigure.widthMax +" > x > "+ toConfigure.widthMin,
      };
      return;
    }

    if ((toConfigure.thickness.indexOf(details.thickness))) {
      error = {
        msg: "Zła grubość",
      };
      return;
    }

    const newConfiguration = {
      idProduct: product.id,
      specification: details,
      price: price,
      order: true,
      amount: amount,
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

  $: if (product.price) {
    price = product.price;
  }

  function closeModal() {
    error = undefined;
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
      <div class="input-container">
        <label for="wysokosc">Wysokość [mm]:</label>
        <input
          type="number"
          id="wysokosc"
          bind:value={details.height}
          min= {toConfigure.heightMin}
          max= {toConfigure.heightMax}
        />
      </div>

      <div class="input-container">
        <label for="szerokosc">Szerokość [mm]:</label>
        <input
          type="number"
          id="szerokosc"
          bind:value={details.width}
          min= {toConfigure.widthMin}
          max= {toConfigure.widthMax}
        />
      </div>
      <div class="input-container">
        <label for="grubosc">Grubość [mm]:</label>
        <select id="grubosc" bind:value={details.thickness}>
          {#each toConfigure.thickness as g}
            <option value={g}>{g}</option>
          {/each}
        </select>
      </div>

      <div class="input-container">
        <label for="kolor">Kolor:</label>
        <select id="kolor" bind:value={details.color}>
          {#each toConfigure.color as k}
            <option value={k}>{k}</option>
          {/each}
        </select>
      </div>
      <div
        class="lg:flex item-center lg:space-x-10 pt-4"
        style="position: relative;"
      >
        <div class="amount-container">
          <Button on:click={decreaseAmount} style="background:#73AD21;">
            Odejmnij
          </Button>
          <input
            type="number"
            id="amount"
            min="0"
            max="999"
            bind:value={amount}
            on:input={handleInput}
            class="py-1 px-2 border rounded-md"
            style="width: 80px;"
          />
          <Button on:click={increaseAmount} style="background:#73AD21;">
            Dodaj
          </Button>

          <div class="">
            {price} zł
          </div>
        </div>
      </div>
    </form>

    <Button
      type="button"
      class=""
      style="background:#73AD21;"
      on:click={saveConfigurations}>Zapisz Konfigurację</Button
    >
  </div>
</div>

<style>
.input-container {
  display: flex;
  align-items: center;
}

.input-container label {

  flex: 1;
  text-align: left;
  margin-right: 10px; 
}

.input-container input, select {
  flex: 1;
}

  .amount-container {
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .update-form {
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

  label {
    margin-top: 10px;
  }

  button {
    display: block;
    margin-top: 10px;
  }

  select,
  input {
    display: inline-block;
    margin-top: 10px;
    border: 6px solid #73ad21;
    border-radius: 25px;
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
