<script>
	import { onMount, onDestroy } from 'svelte';
	import { browser } from '$app/environment';
	import '../app.css';
	let sliderLocked = true;
	let sliderRef, handleRef, imageWrapperRef;

	function onMouseMove(event) {
		if (sliderLocked) return;
		updateHandlePosition(event);
	}

	function updateHandlePosition(event) {
		const sliderLeftX = sliderRef.offsetLeft;
		const sliderWidth = sliderRef.clientWidth;
		const sliderHandleWidth = handleRef.clientWidth;

		let mouseX = (event.clientX || event.touches[0].clientX) - sliderLeftX;
		if (mouseX < 0) {
			mouseX = 0;
		} else if (mouseX > sliderWidth) {
			mouseX = sliderWidth;
		}

		imageWrapperRef.style.width = `${((1 - mouseX / sliderWidth) * 100).toFixed(4)}%`;
		handleRef.style.left = `calc(${((mouseX / sliderWidth) * 100).toFixed(4)}% - ${sliderHandleWidth / 2}px)`;
	}

	function onMouseDown() {
		sliderLocked = false;
		// updateHandlePosition(event);
	}

	function onMouseUp() {
		if (!sliderLocked) sliderLocked = true;
	}

	function onMouseLeave(event) {
		if (!sliderLocked) updateHandlePosition(event);
	}

	onMount(() => {
		if (browser) window.addEventListener('mouseup', onMouseUp);
	});

	onDestroy(() => {
		if (browser) window.removeEventListener('mouseup', onMouseUp);
	});
</script>

<div class="image-comparison-slider" on:mousemove={onMouseMove} on:mousedown={onMouseDown} on:mouseleave={onMouseLeave} on:touchstart={onMouseDown} on:touchend={onMouseUp} on:touchmove={onMouseMove} bind:this={sliderRef}>
	<img src="https://www.chatpim.com/media/test/3-2.jpg" alt="after" />
	<div class="img-wrapper" bind:this={imageWrapperRef}>
		<img src="https://www.chatpim.com/media/test/3-2-.jpg" alt="before" />
	</div>
	<span class="label label-before">Before</span>
	<span class="label label-after">After</span>
	<div class="handle" bind:this={handleRef}>
		<div class="handle-line"></div>
		<div class="handle-circle">
			<svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" width="22" height="22">
				<polygon fill="#FFF" points="6.36,18 0,12 6.36,6" />
				<polygon fill="#FFF" points="17.64,18 24,12 17.64,6" />
			</svg>
		</div>
		<div class="handle-line"></div>
	</div>
</div>

<style>
	:root {
		--slider-width: min(80vw, 768px);
		--handle-width: 48px;
	}

	.image-comparison-slider {
		position: relative;
		width: var(--slider-width);
		overflow: hidden;
		border-radius: 0.5rem;
		box-shadow: -7px 5px 16px 1px rgba(56, 86, 122, 0.6);
	}

	img {
		display: block;
		width: var(--slider-width);
		height: auto;
		max-height: 80vh;
		object-fit: cover;
		pointer-events: none;
		user-select: none;
	}

	.img-wrapper {
		position: absolute;
		top: 0;
		right: 0;
		width: 50%;
		height: 100%;
		overflow: hidden;
		z-index: 1;
	}

	.img-wrapper img {
		position: absolute;
		top: 0;
		right: 0;
		height: 100%;
	}

	.label {
		width: 71px;
		position: absolute;
		top: 1rem;
		z-index: 3;
		color: #fff;
		background-color: rgba(0, 0, 0, 0.33);
		border-radius: 0.25rem;
		padding: 0.5rem 0.75rem;
		font-size: 0.85rem;
		text-align: center;
		letter-spacing: 1px;
		user-select: none;
		opacity: 0;
		transition: 0.25s cubic-bezier(0.68, 0.26, 0.58, 1.22);
	}

	.image-comparison-slider:hover .label {
		opacity: 1;
	}

	.label-before {
		right: 1rem;
	}

	.label-after {
		left: 1rem;
	}

	.handle {
		position: absolute;
		top: 0;
		left: calc(50% - var(--handle-width) / 2);
		width: var(--handle-width);
		height: 100%;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		user-select: none;
		z-index: 2;
		cursor: pointer;
	}

	.handle-line {
		width: 3px;
		flex-grow: 1;
		background-color: #fff;
	}

	.handle-circle {
		width: var(--handle-width);
		height: var(--handle-width);
		color: #fff;
		border: 3px solid #fff;
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: space-evenly;
		background-color: rgba(0, 0, 0, 0);
	}

	@media (max-width: 768px) {
		:root {
			--slider-width: 90vw;
		}
	}
</style>
