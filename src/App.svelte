<script>
	import ImageEditor from "./ImageEditor.svelte";
</script>

<style>
	header {
		color: #000;
		font-size: 20pt;
		font-weight: bold;
		padding: 15px 0px 15px 15px;
		margin-bottom: 20px;
		background: antiquewhite;
		box-shadow: 0 0 5px rgba(0,0,0,0.5);
	}
	
	.image-editor {
		padding: 0px 20px;
	}
</style>

<header>Create image with text</header>

<div class="image-editor">
	<ImageEditor />
</div>