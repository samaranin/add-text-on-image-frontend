<script>
    import { onMount } from 'svelte';

    export let image_url = "/sources/label/393-960.png";
    export let height = "120px";
</script>

<img height={height} src="{image_url}" alt="label" />