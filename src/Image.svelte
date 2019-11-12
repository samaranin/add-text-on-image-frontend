<script>
    import { onMount } from 'svelte';

    export let image_url = "/sources/label/393-960.png";
    export let label = "";
</script>

<label>{label}</label>
<img src="{image_url}" alt="label" />