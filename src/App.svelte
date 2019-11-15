<script>
	import ImageEditor from "./ImageEditor.svelte";
	import Footer from "./Footer.svelte";
</script>

<style>
	header {
		color: #fff;
		font-size: 20pt;
		font-weight: 500;
		padding: 10px 0px 10px 15px;
		margin-bottom: 20px;
		background: rgba(61, 153, 112, 0.9);
		box-shadow: 0 0 5px rgba(0,0,0,0.5);
	}

	.page-container {
		overflow: hidden;
		position: relative;
  		min-height: 100vh;
		background: whitesmoke;
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
	<header>NUWEE library helper</header>

	<div class="image-editor">
		<ImageEditor />
	</div>

	<Footer />
</div>