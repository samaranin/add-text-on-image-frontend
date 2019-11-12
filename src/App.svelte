<script>
	import Dropdown from './Dropdown.svelte';
	import Image from './Image.svelte';

	const server_url = "http://localhost:5000";
</script>

<style>
	h1 {
		color: #000;
	}
</style>

<h1>Create image with text</h1>

<Dropdown label="Fonts:" list_url={server_url+"/api/fonts/"}/>
<Dropdown label="Backgrounds:" list_url={server_url+"/api/backgrounds/"} />
<Dropdown label="Labels:" list_url={server_url+"/api/labels/"} />
<Image  label="Label Image:" image_url={server_url+"/sources/labels/393-960.png"} />