<script>
    import Image from "./Image.svelte"

    export let join = "";
    export let readyImage = "";
    export let labelImage = "";
    export let backgroundImage = "";
</script>

<style>
    .preview-wrapper {
        box-shadow: 0 0 5px rgba(0,0,0,0.5);
    }

    .raw-preview, .ready-preview {
		display: flex;
		justify-content: center;
		padding: 15px;
		margin-bottom: 15px;
        background: #fff;
	}

	.preview-header {
		text-align: center;
		font-size: 15pt;
		font-weight: bold;
		padding: 10px;
		background: rgba(61, 153, 112, 0.9);
        box-shadow: 0px 2px 5px rgba(0,0,0,0.5);
        color: #fff;
	}
</style>

<div class="preview-wrapper raw-preview-wrapper">
    <div class="preview-header">Selected images</div>
    <div class="raw-preview">
        {#if join === "left"}
            <Image 
                height="200px" 
                imgClass="preview-img"
                image_url={labelImage} 
            />
            <Image 
                height="200px" 
                imgClass="preview-img"
                image_url={backgroundImage} 
            />
        {:else}
            <Image 
                height="200px" 
                imgClass="preview-img"
                image_url={backgroundImage} 
            />
            <Image 
                height="200px" 
                imgClass="preview-img"
                image_url={labelImage}  
            />
        {/if}
    </div>
</div>

<div class="preview-wrapper">
    <div class="preview-header">Ready image</div>
    <div class="ready-preview">
        <Image 
            height="none" 
            imgClass="ready-img"
            image_url={readyImage} 
        />
    </div>
</div>