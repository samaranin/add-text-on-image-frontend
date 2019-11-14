<script>
    import Image from "./Image.svelte"

    export let join = "";
    export let readyImage = "";
    export let labelImage = "";
    export let backgroundImage = "";
</script>

<style>
    .raw-preview, .ready-preview {
		display: flex;
		justify-content: center;
		padding: 15px;
		margin-bottom: 15px;
		box-shadow: 0 0 5px rgba(0,0,0,0.5);
	}

	.preview-header {
		text-align: center;
		font-size: 15pt;
		font-weight: bold;
		padding: 10px;
		margin-block-start: 0;
		margin-block-end: 0;
		background: floralwhite;
		box-shadow: 0px -3px 10px rgba(0,0,0,0.5);
	}
</style>

<p class="preview-header">Selected images:</p>
<div class="raw-preview">
    {#if join === "left"}
        <Image 
            height="200px" 
            image_url={labelImage} 
        />
        <Image 
            height="200px" 
            image_url={backgroundImage} 
        />
    {:else}
        <Image 
            height="200px" 
            image_url={backgroundImage} 
        />
        <Image 
            height="200px" 
            image_url={labelImage}  
        />
    {/if}
    
</div>

<p class="preview-header">Ready image:</p>
<div class="ready-preview">
    <Image 
        height="none" 
        imgClass="ready-img"
        image_url={readyImage} 
    />
</div>