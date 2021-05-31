<script lang="ts">
	import { onMount } from "svelte";
	import Equation from "./Equation.svelte";
	import Number from "./Number.svelte";
	let value = 8;
	let spareCount = 0;
</script>

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