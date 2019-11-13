<script>
	import Dropdown from './Dropdown.svelte';
	import Image from './Image.svelte';
	import Input from './Input.svelte';

	let selected_values = [];

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

	function handleValueChange(event) {
		getImageSize(event.detail.value);
	}
</script>

<style>
	h1 {
		color: #000;
	}

	.container {
		width: 100%;
	}

	.image-editor {
		width: 39%;
		display: inline-block;
	}

	.preview {
		float: right;
		width: 59%;
	}

	.raw-preview, .ready-preview {
		display: flex;
		justify-content: center;
		padding: 10px;
	}

	.preview-header {
		text-align: center;
		font-size: 15pt;
		font-weight: bold;
	}
	
	.image-size-inputs {
		width: 100%;
		display: inline-flex;
	}

	.show-preview {
		float: left;
	}

	.download {
		float: right;
	}
</style>

<h1>Create image with text</h1>


<div class="container">
	<form class="image-editor">
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
			on:select_value={handleValueChange}	
		/>
		<!--Selected label-->
		<Dropdown 
			label="Label:" 
			list_url={server_url+"/api/labels/"} 
			bind:selected={selected_values["label_image"]} 
		/>
		<hr />
		<!--Header inputs-->
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
		<hr />

		<!--Paragraph inputs-->
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
		<hr />
		<!--Footer inputs-->
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
		<hr />
		<!--Image size-->
		<div class="image-size-inputs">
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
		<hr />
		<!--Action buttons-->
		<div class="action-buttons">
			<button class="show-preview">Show preview</button>
			<button class="download">Download</button>
		</div>
	</form>

	<div class="preview">
		<p class="preview-header">Selected images:</p>
		<div class="raw-preview">
			<Image 
				height="150px" 
				image_url={server_url+"/sources/labels/"+selected_values["label_image"]} 
			/>
			<Image 
				height="150px" 
				image_url={server_url+"/sources/backgrounds/"+selected_values["background_image"]} 
			/>
		</div>
		<p class="preview-header">Ready image:</p>
		<div class="ready-preview">
			<Image 
				height="400px" 
				image_url={server_url+"/sources/backgrounds/"+selected_values["background_image"]} 
			/>
		</div>
	</div>
</div>