<script lang="ts">
	import {onMount} from 'svelte';
	import Equation from './Equation.svelte';
	import Number from './Number.svelte';
	let value = 8;
	let spareCount = 0;
</script>

<svelte:head>
	<link rel="stylesheet" href="https://fonts.googleapis.com/css?family=Rajdhani">
	<style>
		body {
			font-family: 'Rajdhani', sans-serif;
		}
	</style>
</svelte:head>

<main>
	<Equation x={9} y={9} r={78} op="-" maxMoves={1} />
</main>

<style>
	main {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
		justify-content: center;
	}
</style>