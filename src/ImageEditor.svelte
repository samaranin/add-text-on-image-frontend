<script>
	import Dropdown from './Dropdown.svelte';
	import Image from './Image.svelte';
	import Input from './Input.svelte';
	import Radiobutton from './Radiobutton.svelte';

	let selected_values = [];
	let resultImagePath = "";

	const server_url = "http://localhost:5000";

	async function getImageSize(selected_image){ 
		const response = await fetch(
			server_url+"/api/get_image_size/", 
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
		const cleared = await fetch(server_url + "/api/remove_temp_files/");

		// write text on image
		const data = {
			"background_image": selected_values["background_image"], // <- must be on server in sources/backgrounds   
			"header": selected_values["header"],    
			"paragraph": selected_values["paragraph"],    
			"footer": selected_values["footer"],   
			"font_name": selected_values["font_name"],     // <- must be on server in fonts/
			"width": selected_values["width"],    
			"height": selected_values["height"],    
			"text_width_header": selected_values["text_width_header"],  
			"font_size_header": selected_values["font_size_header"],    
			"text_width_paragraph": selected_values["text_width_paragraph"],    
			"font_size_paragraph": selected_values["font_size_paragraph"],    
			"font_size_footer": selected_values["font_size_footer"]
		};

		const response = await fetch(
			server_url+"/api/write_text/", 
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
</script>

<style>
	.container {
		width: 100%;
	}

	.image-editor {
		width: 39%;
		display: inline-block;
    }
    
    .editor-section {
        border-bottom: 1px solid rgba(0,0,0,0.5);
        margin-bottom: 10px;
    }

	.preview {
		float: right;
		width: 59%;
	}

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
	
	.image-size-inputs {
		width: 100%;
		display: inline-flex;
		display: none;
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
			display: block;
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
	<form class="image-editor">
        <div class="editor-section">
            <!--Selected font-->
            <Dropdown 
                label="Font:" 
                list_url={server_url+"/api/fonts/"} 
                bind:selected={selected_values["font_name"]} 
            />

            <!--Selected background-->
            <Dropdown 
                label="Background:" 
                list_url={server_url+"/api/backgrounds/"} 
                bind:selected={selected_values["background_image"]} 
                on:change={handleValueChange}	
            />

            <!--Selected label-->
            <Dropdown 
                label="Label:" 
                list_url={server_url+"/api/labels/"} 
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
		</div>

		<!--Paragraph inputs-->
		<div class="editor-section">
            <Input 
                label="Paragraph:" 
                placeholder="Paragraph text" 
                bind:value={selected_values["paragraph"]} 
                disabled="{false}" 
            />

            <Input 
                label="Paragraph text width:" 
                placeholder="30" 
                bind:value={selected_values["text_width_paragraph"]} 
                disabled="{false}" 
            />

            <Input 
                label="Paragraph font size:" 
                placeholder="30" 
                bind:value={selected_values["font_size_paragraph"]} 
                disabled="{false}"
            />
		</div>

		<!--Footer inputs-->
        <div class="editor-section">
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
				<a class="download" href="{server_url+"/"+resultImagePath}" download="image-preview">Download</a>
			{:else}
				<a class="disabled-button" href="#">Download</a>
			{/if}
		</div>
	</form>

	<div class="preview">
		<p class="preview-header">Selected images:</p>
		<div class="raw-preview">
			{#if selected_values["join"] === "left"}
				<Image 
					height="200px" 
					image_url={server_url+"/sources/labels/"+selected_values["label_image"]} 
				/>
				<Image 
					height="200px" 
					image_url={server_url+"/sources/backgrounds/"+selected_values["background_image"]} 
				/>
			{:else}
				<Image 
					height="200px" 
					image_url={server_url+"/sources/backgrounds/"+selected_values["background_image"]} 
				/>
				<Image 
					height="200px" 
					image_url={server_url+"/sources/labels/"+selected_values["label_image"]} 
				/>
			{/if}
			
		</div>
		<p class="preview-header">Ready image:</p>
		<div class="ready-preview">
			<Image 
				height="none" 
				imgClass="ready-img"
				image_url={server_url+"/"+resultImagePath} 
			/>
		</div>
	</div>
</div>