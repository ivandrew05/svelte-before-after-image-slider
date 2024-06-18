<script>
	import BeforeAfterSlider from '$lib/BeforeAfterSlider.svelte';
	let circleSize = '44px';
	let lineWidth = '3px';
	let arrowSize = '22px';
	let sliderColor = 'white';
	let labelLeft = 'After';
	let labelRight = 'Before';
	let imageLeftSrc = 'https://www.chatpim.com/media/example_image/2-3.jpg';
	let imageRightSrc = 'https://www.chatpim.com/media/example_image/2-3-.jpg';
</script>

<main>
	<div class="container">
		<BeforeAfterSlider {circleSize} {lineWidth} {arrowSize} {sliderColor} {labelLeft} {labelRight} {imageLeftSrc} {imageRightSrc} />
	</div>
</main>

<style>
	:global(body) {
		margin: 0;
		padding: 0;
		height: 100vh;
		font-family: Helvetica, sans-serif;
	}

	main {
		width: 100%;
		height: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
	}

	.container {
		width: 512px;
		height: 768px;
		overflow: hidden;
		border-radius: 8px;
	}
</style>
