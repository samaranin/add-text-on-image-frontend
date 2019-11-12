<script>
    import { onMount } from 'svelte';

    export let list_url = "http://localhost:5000/api/fonts/";
    export let label = "";
    
    let items = [];
  
    // async data fetching function
    const fetchFonts = async (data, component) => {
        const response = await fetch(list_url);
        const json = await response.json();
        items = json["data"];
    }

    onMount(async () => fetchFonts());
</script>

<label>{label}</label>
<select>
{#each items as item}
    <option value="{item}">
        {item}
    </option>
{/each}
</select>