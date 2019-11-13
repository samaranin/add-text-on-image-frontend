<script>
    import { onMount } from 'svelte';

    export let image_url = "";
    export let height = "120px";
</script>

{#if image_url.includes(".")}
    <img height={height} src="{image_url}" alt="label" />
{:else}
    <p>waiting for image...</p>
{/if}