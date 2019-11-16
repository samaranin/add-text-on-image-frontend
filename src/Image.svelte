<script>
    import { onMount } from 'svelte';

    export let image_url = "";
    export let height = "120px";
    export let imgClass = "preview";
</script>

<style>
.ready-img {
    max-width: 100%;
    max-height: 45.4vh;
    min-height: 45.4vh;
    height: auto;
    margin: auto;
    object-fit: scale-down;
    /* border: 1px solid #ddd; */
    /* border-radius: 4px; */
    padding: 5px;
}

.preview-img {
    max-width: 100%;
    max-height: 18vh;
    height: auto;
    margin: 0;
    object-fit: scale-down;
    /* border: 1px solid #ddd; */
    /* border-radius: 4px; */
    padding: 0;
}
</style>

{#if image_url.includes(".png") || image_url.includes(".jpg")}
    <img class={imgClass} height={height} src="{image_url}" alt="label" />
{:else}
    <p>waiting for image...</p>
{/if}