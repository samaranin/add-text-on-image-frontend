<script>
	import Dropdown from './Dropdown.svelte';
	import Image from './Image.svelte';

	let selected_values = [];

	const server_url = "http://localhost:5000";
</script>

<style>
	h1 {
		color: #000;
	}

	.container {
		width: 100%;
	}

	.image-editor {
		width: 49%;
		display: inline-block;
	}

	.preview {
		float: right;
		width: 49%;
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
</style>

<h1>Create image with text</h1>


<div class="container">
	<form class="image-editor">
		<!--Selected font-->
		<Dropdown label="Font:" list_url={server_url+"/api/fonts/"} bind:selected={selected_values["font"]} />
		<!--Selected background-->
		<Dropdown label="Background:" list_url={server_url+"/api/backgrounds/"} bind:selected={selected_values["backround"]} />
		<!--Selected label-->
		<Dropdown label="Label:" list_url={server_url+"/api/labels/"} bind:selected={selected_values["label"]} />
	</form>

	<div class="preview">
		<p class="preview-header">Selected images:</p>
		<div class="raw-preview">
			<Image height="150px" image_url={server_url+"/sources/labels/"+selected_values["label"]} />
			<Image height="150px" image_url={server_url+"/sources/backgrounds/"+selected_values["backround"]} />
		</div>
		<p class="preview-header">Ready image:</p>
		<div class="ready-preview">
			<Image height="400px" image_url={server_url+"/sources/backgrounds/"+selected_values["backround"]} />
		</div>
	</div>
</div>