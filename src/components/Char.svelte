<script>
	import { onMount, createEventDispatcher } from 'svelte';
	import Matchstick from './Matchstick.svelte';
	import includes from 'lodash-es/includes';
	
	export let width = 100;
	export let height = 200;
	export let strokes = [];
	export let activeStrokes = strokes;
	export let lockedStrokes = [];
	export let readonly = true;
	export let name = undefined;
	
	const dispatch = createEventDispatcher();
	let el;
	onMount(() => {
		const observer = new ResizeObserver(() => {
			const w = el?.getBoundingClientRect()?.width;
			const h = el?.getBoundingClientRect()?.height;
			if (w > h / 2) {
				height = h;
				width = h / 2;
			} else {
				width = w;
				height = w * 2;
			}
		}).observe(el);
	});

	function onClick(stroke) {
		dispatch('click', { el, stroke, name })
	}
</script>

<wrapper bind:this={el}>
	<char
		style="
			--width: {width}px;
			--height: {height}px;
			--strikeWidth: {width / 10}px;
		--strikeHorizontalLength: {width / 1.25}px;
			--strikeVerticalLength: {(height - (width / 10)) / 2}px;
		"
	>
		{#each strokes as stroke}
			<stroke
				class="{stroke}"
				class:clickable={!readonly && !includes(lockedStrokes, stroke)}
				class:active={includes(activeStrokes, stroke)}
				on:click={!readonly && !includes(lockedStrokes, stroke) && (() => onClick(stroke))}
			>
				<Matchstick />		
			</stroke>
		{/each}
	</char>
</wrapper>

<style>
	wrapper {
		display: flex;
		justify-content: center;
		align-items: center;
	}
	char {
		display: block;
		position: relative;
		width: var(--width);
		height: var(--height);
	}
	stroke {
		position: absolute;
		display: block;
		height: var(--strikeVerticalLength);
		width: var(--strikeWidth);
		top: 0;
		left: 0;
		opacity: 0.1;
	}
	stroke.clickable {
		cursor: pointer;
	}
	stroke.active {
		opacity: 1;
	}
	char :global(stroke.a) {
		transform: rotate(90deg) translateY(calc(0px - var(--strikeHorizontalLength) - var(--strikeWidth)));
		transform-origin: top left;
		height: var(--strikeHorizontalLength);
	}
	char :global(stroke.b) {
		transform: translateY(5px);
	}
	char :global(stroke.c) {
		transform: translateY(5px);
		left: auto;
		right: 0;
	}
	char :global(stroke.d) {
		transform: rotate(90deg) translate(calc(var(--strikeVerticalLength)), calc(0px - var(--strikeHorizontalLength) - var(--strikeWidth)));
		transform-origin: top left;
		height: var(--strikeHorizontalLength);
	}
	char :global(stroke.e) {
		transform: translateY(calc(var(--strikeVerticalLength) + var(--strikeWidth) / 2));
	}
	char :global(stroke.f) {
		transform: translateY(calc(var(--strikeVerticalLength) + var(--strikeWidth) / 2));
		left: auto;
		right: 0;
	}
	char :global(stroke.g) {
		transform: rotate(90deg) translate(calc(0px - var(--strikeWidth) * 1), calc(0px - var(--strikeWidth)));
		transform-origin: bottom left;
		top: auto;
		bottom: 0;
		height: var(--strikeHorizontalLength);
	}
	char :global(stroke.h) {
		transform: translate(calc(var(--strikeHorizontalLength) / 2 + var(--strikeWidth) / 2), calc(var(--strikeVerticalLength) / 2 + var(--strikeWidth) / 2));
	}
	char :global(stroke.i) {
		transform: rotate(90deg) translate(calc(var(--strikeVerticalLength) - 10px), calc(0px - var(--strikeHorizontalLength) - var(--strikeWidth)));
		transform-origin: top left;
		height: var(--stickHorizontalLength);
	}
	char :global(stroke.j) {
		transform: rotate(90deg) translate(calc(var(--strikeVerticalLength) + 10px), calc(0px - var(--strikeHorizontalLength) - var(--strikeWidth)));
		transform-origin: top left;
		height: var(--stickHorizontalLength);
	}
</style>
