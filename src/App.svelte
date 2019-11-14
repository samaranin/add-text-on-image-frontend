<script>
	import ImageEditor from "./ImageEditor.svelte";
	import Footer from "./Footer.svelte";
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

	.page-container {
		overflow: hidden;
		display: block;
		position: relative;
		padding-bottom: 100px; /* height of your footer */
	}

	.image-editor {
		padding: 0px 20px;
	}

	@media (max-width: 640px) {
		.page-container {
			padding-bottom: 50px; /* height of your footer */
		}
	}
</style>


<div class="page-container">
	<header>Create image with text</header>

	<div class="image-editor">
		<ImageEditor />
	</div>

	<Footer />
</div>