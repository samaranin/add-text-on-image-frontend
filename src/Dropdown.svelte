<script>
    import { onMount } from 'svelte';

    export let list_url = "http://localhost:5000/api/fonts/";
    export let label = "";
    export let selected = "";

    let items = [];
  
    // async data fetching function
    const fetchFonts = async (data, component) => {
        const response = await fetch(list_url);
        const json = await response.json();
        items = json["data"];
        selected = items[0];
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
