<script>
	let containerRef;
	export let circleSize = '44px';
	export let lineWidth = '3px';
	export let arrowSize = '22px';
	export let sliderColor = 'white';
	export let labelLeft = 'After';
	export let labelRight = 'Before';
	export let imageLeftSrc = 'https://www.chatpim.com/media/test/2-3.jpg';
	export let imageRightSrc = 'https://www.chatpim.com/media/test/2-3-.jpg';

	function onInput(e) {
		containerRef.style.setProperty('--position', `${e.target.value}%`);
	}
</script>

<div class="container" bind:this={containerRef} style="--circle-size: {circleSize}; --line-width:{lineWidth}; --slider-color:{sliderColor}">
	<div class="image-container">
		<img class="slider-image image-left" src={imageLeftSrc} alt="left" />
		<img class="slider-image image-right" src={imageRightSrc} alt="right" />
	</div>
	<span class="label label-left">{labelLeft}</span>
	<span class="label label-right">{labelRight}</span>
	<input class="slider" type="range" min="0" max="100" step="0.5" value="50" aria-label="percentage of photo shown" on:input={onInput} />
	<div class="slider-line top" aria-hidden="true"></div>
	<div class="slider-line bottom" aria-hidden="true"></div>
	<div class="slider-circle" aria-hidden="true">
		<svg viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg" width={arrowSize} height={arrowSize}>
			<polygon fill={sliderColor} points="6.36,18 0,12 6.36,6" />
			<polygon fill={sliderColor} points="17.64,18 24,12 17.64,6" />
		</svg>
	</div>
</div>

<style>
	.container {
		width: 100%;
		height: 100%;
		position: relative;
		overflow: hidden;
		--position: 50%;
	}

	.image-container {
		position: relative;
		width: 100%;
		height: 100%;
	}

	.slider-image {
		width: 100%;
		height: 100%;
		object-fit: contain;
		position: absolute;
		top: 0;
	}

	/* .image-left {
		clip-path: polygon(0 0, var(--position) 0, var(--position) 100%, 0 100%);
	} */

	.image-right {
		clip-path: polygon(var(--position) 0, 100% 0, 100% 100%, var(--position) 100%);
	}

	.label {
		position: absolute;
		top: 1rem;
		color: #fff;
		background-color: rgba(0, 0, 0, 0.3);
		border-radius: 0.25rem;
		padding: 0.5rem 0.75rem;
		font-size: 0.85rem;
		text-align: center;
		letter-spacing: 1px;
		user-select: none;
		opacity: 0;
		transition: 0.25s cubic-bezier(0.68, 0.26, 0.58, 1.22);
		z-index: 10;
	}

	.container:hover .label {
		opacity: 1;
	}

	.label-left {
		left: 1rem;
	}

	.label-right {
		right: 1rem;
	}

	.slider {
		position: absolute;
		inset: 0;
		cursor: pointer;
		opacity: 0;
		/* for Firefox */
		width: 100%;
		height: 100%;
	}

	.slider-line {
		position: absolute;
		inset: 0;
		width: var(--line-width);
		height: 100%;
		background-color: var(--slider-color);
		left: calc(var(--position) - var(--line-width) / 2);
		pointer-events: none;
		box-shadow: 0px 0px 12px rgba(51, 51, 51, 0.5);
	}

	.top {
		transform: translateY(calc(-50% - var(--circle-size) / 2));
	}

	.bottom {
		transform: translateY(calc(50% + var(--circle-size) / 2));
	}

	.slider-circle {
		position: absolute;
		border: var(--line-width) solid;
		color: var(--slider-color);
		width: var(--circle-size);
		height: var(--circle-size);
		border-radius: 100%;
		display: grid;
		place-items: center;
		top: 50%;
		left: var(--position);
		transform: translate(-50%, -50%);
		pointer-events: none;
		box-shadow: 0px 0px 12px rgba(51, 51, 51, 0.5);
	}
</style>
