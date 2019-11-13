<script>
    import { onMount } from 'svelte';

    export let image_url = "";
    export let height = "120px";
    export let imgClass = "preview";
</script>

<style>
.ready-img {
    max-width: 100%;
    max-height: 50vh;
    height: auto;
    margin: auto;
    object-fit: scale-down;
}
</style>

{#if image_url.includes(".")}
    <img class={imgClass} height={height} src="{image_url}" alt="label" />
{:else}
    <p>waiting for image...</p>
{/if}