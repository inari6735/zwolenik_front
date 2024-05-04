  <script>
    import {  Button } from 'flowbite-svelte';
 
     export let product = undefined;
 
 
     let wysokosc = 100;
     let szerokosc = 100; 
     const grubosci = [18, 19, 22];
     let grubosc = grubosci[0];
     const kolory = ['Czerwony', 'Zielony', 'Niebieski', 'Czarny'];
     let kolor = kolory[0];
     let specka = {
           "Wysokość": {
             "Unit": "mm",
             "Minimalna wysokość wysokość": 250,
             "Maksymalna wysokość": 1000,
             "cost": 10
           },
           "Grubość": {
             "Unit": "mm",
             "X": 10,
             "Y": 20,
             "Z": 30,
             "costX": 0,
             "costY": 5,
             "costZ": 10
           }
         }
   
 
     function saveConfiguration() {
       console.log({ wysokosc, szerokosc, grubosc, kolor });
     }
   </script>
 
 <div class="container  ">
 
     <div class="image-container">
       <img src="/SW32313_13242.png" alt="" class="product-image" />
     </div>
     <div class="flex">
    
     <h1> {product.name}</h1> 
   
     <div class="flex form-container">
      
       <!-- <h2>Konfigurator Produktu</h2> -->
       <form on:submit|preventDefault={zapiszKonfiguracje}>
         <!-- {#if product.specification}
           {#each Object.entries(product.specification) as [key, value]}
             <label for="{key}">{key} [mm]:</label>
             <input type="text" name="{key}" value="{value}" min="50" max="2750" />
           {/each}
         {/if} -->
 
 
           <!-- {#each Object.entries(specka) as [key, value]}
             <div>{key} {value.Unit}</div>
             {#each Object.entries(value) as [k, v]}S
 
               <label for={k} />
               <input type="text" name="{k}" value="{v}" min="50" max="2750" />
 
             {/each}
           {/each}
 
           <select id="kolor" bind:value={kolor}>
             {#each kolory as k}
               <option value={k}>{k}</option>
             {/each}
           </select>  -->
 
         
   
         <h1 for="szerokosc">Szerokość [mm]:</h1>
         <input type="number" id="szerokosc" bind:value={szerokosc} min="50" max="2000" />
   
         <label for="grubosc">Grubość [mm]:</label>
         <select class="" id="grubosc" bind:value={grubosc}>
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
      
     </div>
 
 </div>  
    <Button type="submit">Zapisz Konfigurację</Button>
   </div>
   
   <style>
     .container {
       padding: 50px;
       margin: auto;
       border-radius: 25px;
       border: 6px solid #73AD21;
       display: flex;
       justify-content: center;
       gap: 20px;
       align-items: center;
       flex-direction: row; /* Domyślnie ustawione na wiersz */
     }
     .image-container {
       min-width: 400px;
       display: flex;
       justify-content: center;
       flex: 1;
     }
     .form-container {
       padding: 30px;
       border-radius: 25px;
       border: 6px solid #73AD21;
       display: flex;
       flex-direction: column;
       flex: 1;
     }
     .product-image {
       max-width: 100%;
       height: auto;
     }
     label, input, select, button {
       display: block;
       margin: 10px 0;
     }
     .form-style {
       width: 100%;
       border-radius: 25px;
       border: 6px solid #73AD21;
     }
   
     /* Media Queries */
     @media (max-width: 640px) {
       .container {
         flex-direction: column; /* Zmienia układ na pionowy */
       }
       .image-container {
         min-width: auto; /* Usuwa minimalną szerokość dla mniejszych ekranów */
         width: 100%; /* Ustawia szerokość kontenera obrazu na 100% */
       }
     }
   </style>
   