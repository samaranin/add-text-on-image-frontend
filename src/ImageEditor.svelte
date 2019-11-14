<script>
	import Dropdown from './Dropdown.svelte';
	import Image from './Image.svelte';
	import Input from './Input.svelte';
    import Radiobutton from './Radiobutton.svelte';
    import Preview from './Preview.svelte';
    import {server_url} from "./env.js";

	let selected_values = [];
	let resultImagePath = "";

	async function getImageSize(selected_image){ 
		const response = await fetch(
			urlGenerator("/api/get_image_size/"), 
			{
				headers: {
					'Accept': 'application/json',
					'Content-Type': 'application/json'
				},
				method: "POST",
				body: JSON.stringify({"image_type": "background", "image_name": selected_image})
			}
		);
        const json = await response.json();
		selected_values["width"] = json["width"];
		selected_values["height"] = json["height"];
	}

	async function getImageWithText() {
		//clear previous generated images
		const cleared = await fetch(urlGenerator("/api/remove_temp_files/"));

		// write text on image
		const data = {
			"background_image": selected_values["background_image"], // <- must be on server in sources/backgrounds   
			"header": selected_values["header"],        
			"footer": selected_values["footer"],   
			"font_name": selected_values["font_name"],     // <- must be on server in fonts/
			"width": selected_values["width"],    
            "height": selected_values["height"],
            "header_font_name": selected_values["header_font_name"],    
			"text_width_header": selected_values["text_width_header"],  
            "font_size_header": selected_values["font_size_header"],
            "footer_font_name": selected_values["footer_font_name"],
            "font_size_footer": selected_values["font_size_footer"],
            "top_padding": selected_values["top_padding"],
            "bottom_padding": selected_values["bottom_padding"],
		};

		const response = await fetch(
            urlGenerator("/api/write_text/"), 
			{
				headers: {
					'Accept': 'application/json',
					'Content-Type': 'application/json'
				},
				method: "POST",
				body: JSON.stringify(data)
			}
		);
		const json = await response.json();
		return json["data"];
	}

	async function getPreviewImagePath() {
		const image_with_text = await getImageWithText();
		
		// join background and label
		const data = {
			"text_image": image_with_text, 
			"label_image": selected_values["label_image"], 
			"join": selected_values["join"]
		};

		const response = await fetch(
			server_url+"/api/join_images/", 
			{
				headers: {
					'Accept': 'application/json',
					'Content-Type': 'application/json'
				},
				method: "POST",
				body: JSON.stringify(data)
			}
		);

		const json = await response.json();
		return json["data"];
	}

	async function getPreviewImage(){
		resultImagePath = await getPreviewImagePath();
	}

	function handleValueChange(event) {
		getImageSize(event.detail.value);
	}

	function repaintPreview(event) {
		getPreviewImage();
	}

    function urlGenerator(path, image="") {
        return server_url + path + image;
    }
</script>

<style>
	.container {
		width: 100%;
    }
    
    .image-editor {
        width: 39%;
        display: inline-block;
        background: #fff;
        box-shadow: 0 0 5px rgba(0,0,0,0.5);
    }

	.image-editor-form {
        padding: 5px;
    }

    .image-editor-header{
        text-align: center;
        font-size: 15pt;
        font-weight: bold;
        color: #000;
        background: floralwhite;
        box-shadow: 0px 2px 5px rgba(0,0,0,0.5);
        padding: 10px;
    }
    
    .editor-section {
        border-bottom: 1px solid rgba(0,0,0,0.5);
        margin: 10px 0px;
    }

	.preview {
		float: right;
		width: 59%;
	}
	
	.image-size-inputs {
		width: 100%;
		display: none;
    }
    
    .action-buttons {
        padding-top: 5px;
    }

	.show-preview {
		float: left;
	}

	.download {
		float: right;
	}

	@media (max-width: 640px) {
		.image-editor {
			width: 100%;
			display: inline-block;
            margin-bottom: 20px;
		}

		.preview {
			float: left;
			width: 100%;
			display: block;
		}
	}

	button, a  {
		text-decoration: none;
		color: #000;
		border: 1px solid black;
		background: floralwhite;
		cursor: pointer;
	}

	a {
		padding: 6.4px 10px;
    	border-radius: 2px;
	}

	.disabled-button {
		float: right;
		background: #ededed;
		color: #666;
		border: 1px solid #efefef;
		cursor: default;
	}
</style>


<div class="container">
    <div class="image-editor">
        <div class="image-editor-header">Configuration</div>
        <form class="image-editor-form">
            <div class="editor-section">
                <!--Selected background-->
                <Dropdown 
                    label="Background:" 
                    list_url={urlGenerator("/api/backgrounds/")} 
                    bind:selected={selected_values["background_image"]} 
                    on:change={handleValueChange}	
                />

                <!--Selected label-->
                <Dropdown 
                    label="Label:" 
                    list_url={urlGenerator("/api/labels/")} 
                    bind:selected={selected_values["label_image"]} 
                />

                <!--Image join side-->
                <Radiobutton 
                    labelLeft="Place label on left"
                    labelRight="Place label on right"
                    bind:value={selected_values["join"]}
                />
            </div>

            <!--Header inputs-->
                <!--Selected font-->
                <Dropdown 
                    label="Header font:" 
                    list_url={urlGenerator("/api/fonts/")} 
                    bind:selected={selected_values["header_font_name"]} 
                />

            <div class="editor-section">
                <Input 
                    label="Header:" 
                    placeholder="Header text" 
                    bind:value={selected_values["header"]} 
                    disabled="{false}" 
                />

                <Input 
                    label="Header text width:" 
                    placeholder="40" 
                    bind:value={selected_values["text_width_header"]} 
                    disabled="{false}"
                />

                <Input 
                    label="Header font size:" 
                    placeholder="30" 
                    bind:value={selected_values["font_size_header"]} 
                    disabled="{false}" 
                />

                <Input 
                    label="Top padding:" 
                    placeholder="200" 
                    bind:value={selected_values["top_padding"]} 
                    disabled="{false}" 
                />
            </div>

            <!--Footer inputs-->
            <div class="editor-section">
                <!--Selected font-->
                <Dropdown 
                    label="Footer font:" 
                    list_url={urlGenerator("/api/fonts/")} 
                    bind:selected={selected_values["footer_font_name"]} 
                />

                <Input 
                    label="Footer:" 
                    placeholder="Footer text" 
                    bind:value={selected_values["footer"]} 
                    disabled="{false}" 
                />
                <Input 
                    label="Footer font size:" 
                    placeholder="30" 
                    bind:value={selected_values["font_size_footer"]} 
                    disabled="{false}" 
                />

                <Input 
                    label="Bottom padding:" 
                    placeholder="140" 
                    bind:value={selected_values["bottom_padding"]} 
                    disabled="{false}" 
                />
            </div>

            <!--Image size-->
            <div class="image-size-inputs editor-section">
                <Input 
                    label="Width:" 
                    placeholder="960" 
                    bind:value={selected_values["width"]} 
                    disabled="{true}" 
                />
                
                <Input 
                    label="Height:" 
                    placeholder="960" 
                    bind:value={selected_values["height"]} 
                    disabled="{true}" 
                />
            </div>

            <!--Action buttons-->
            <div class="action-buttons">
                <button on:click|preventDefault={getPreviewImage} class="show-preview">Show preview</button>
                {#if resultImagePath != ""}
                    <a class="download" href="{urlGenerator("/",resultImagePath)}" download="image-preview">Download</a>
                {:else}
                    <a class="disabled-button" href="/#">Download</a>
                {/if}
            </div>
        </form>
    </div>

	<div class="preview">
		<Preview 
            join={selected_values["join"]}
            labelImage={urlGenerator("/sources/labels/", selected_values["label_image"])}
            backgroundImage={urlGenerator("/sources/backgrounds/", selected_values["background_image"])}
            readyImage={urlGenerator("/",resultImagePath)}
        />
	</div>
</div>