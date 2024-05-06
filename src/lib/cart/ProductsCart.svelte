<script>
  import { Button } from "flowbite-svelte";

  import { createEventDispatcher } from 'svelte';

    export let value;
    const dispatch = createEventDispatcher();


    function decreaseAmount() {
        if (value.amount > 0) {
            value.amount -= 1;
            dispatch('change', { amount: value.amount });
        }
    }

    function increaseAmount() {
        if (value.amount < 999) {
            value.amount += 1;
            dispatch('change', { amount: value.amount });
        }

    }

    function handleInput(event) {
        value.amount = parseInt(event.target.value, 10);
        if (isNaN(value.amount) || value.amount < 0) {
            value.amount = 0;
        }
        dispatch('change', { amount: value.amount });
    }
    
</script>


  <div class={'p-5 shadow-lg border rounded-md flex-1 order' }>

            <div class={'flex items-center'}>
                <div class={'w-[5rem] h-[5rem] lg:w-[9rem] lg:h-[9rem]'}>
                    <img class={'w-auto h-full object-cover object-top items-center'}
                        src="/SW32313_13242.png" alt={''} />

                </div>
                <!-- {/* dac info o produkcie */} -->
                <div class={'ml-5 space-y-1'}>

                    <p class={'font-semibold'}>Product id : {value.id}</p>

                    <!-- <p class={'opacity-70'}>JAKIES DETALE</p> -->
                    <p class={'opacity-70'}>Wysokość: {value.details.height}</p>
                    <p class={'opacity-70'}>Szerokość: {value.details.width}</p>
                    <p class={'opacity-70'}>Grubość: {value.details.thickness}</p>
                    <!-- <p class={'opacity-70 mt-2'}> Costam</p> -->

                    <p class={'opacity-70 '}>Dac details clienta</p>

                    <div class={'flex space-x-5 items-center text-gray-900 pt-10'}>
                        <p class={'font-semibold'}>Cena: {value.price} zł</p>
                        <!-- <p class={'opacity-50 line-through'}> Opcja promocji</p> -->

                    </div>

                    <div>

                    </div>
                </div>



            </div>
            <!-- {/* PRZYCISK OD ILOSCI */} -->
            <div class='lg:flex item-center lg:space-x-10 pt-4 ' style="position: relative;">

                <div class="content " style="position: absolute; bottom: 0; right: 0;">
                    <Button on:click={decreaseAmount} style="background:#73AD21;"> 

                        Odejmnij
                    </Button>
                    
                    <input type="number" min="0" max="999" value={value.amount} on:input={handleInput} class="py-1 px-2 border rounded-md" style="width: 80px;">
                    <Button on:click={increaseAmount} style="background:#73AD21;">

                        Dadaj
                    </Button>
                    

                </div>

            </div>

        </div>

<style>
    .order {
        width: 100%;
        margin: 20px;
        border-radius: 25px;
        border: 6px solid #73ad21;
    }


</style>
