<script>
    export let value;
</script>

<div class="p-5 order flex">
    <p>Id :{value.id}</p>
    <p>Wysokość: {value.wysokosc}</p>
    <p>Szerokość: {value.szerokosc}</p>
    <p>Grubość: {value.grubosc}</p>
    <p>Kolor: {value.kolor}</p>
    <p>Cena: {value.price}</p>
    <p></p>
</div>

<style>
    .order {
        width: 100%;
        margin: 20px;
        border-radius: 25px;
        border: 6px solid #73ad21;
    }
</style>
