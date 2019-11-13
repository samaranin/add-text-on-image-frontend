<script>
    import { onMount } from 'svelte';

    export let list_url = "http://localhost:5000/api/fonts/";
    export let label = "";
    export let selected = "";
  
    // async data fetching function
    async function fetchFonts() {
        const response = await fetch(list_url);
        const json = await response.json();
        const items = json["data"];
        selected = items[0];
        return items;
    }

    let promise = fetchFonts();

    function getFonts() {
        promise = fetchFonts();
    }

    onMount(async () => fetchFonts());
</script>

<style>
    label, select {
        display: inline;
        padding: 10px;
    }

    select {
        float: right;
        width: 59%;
    }

    div {
        width: 100%;
        display: flex;
        justify-content: center;
        height: auto;
    }

    label {
        width: 40%;
    }
</style>

{#await promise}
    <p>...waiting</p>
{:then items}
    <div>
        <label>{label}</label>
        <select bind:value={selected}>
            {#each items as item}
                <option value="{item}">
                    {item}
                </option>
            {/each}
        </select>
    </div>
{:catch error}
    <p style="color: red">{error.message}</p>
{/await}

