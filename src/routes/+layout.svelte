<script>
	import '../app.css';
	import '@fontsource/noto-sans/700.css';
</script>

<slot />

<style>
	:global(body) {
		font-family: 'Noto Sans', sans-serif;
	}
	:global(body) {
		-webkit-tap-highlight-color: transparent;
	}
</style>
