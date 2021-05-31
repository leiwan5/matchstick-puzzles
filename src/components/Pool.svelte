<script>
	import Matchstick from './Matchstick.svelte';
	import range from 'lodash-es/range';
	export let capacity = 10;
	export let count = 0;
</script>

<spots>
	{#each range(0, capacity) as n}
		<spot class:active={n < count}>
			<matchstick>
				<Matchstick />
			</matchstick>
		</spot>
	{/each}
</spots>

<style>
	spots {
		display: flex;
		flex-wrap: wrap;
		justify-content: center;
	}
	spot {
		height: 15px;		
		width: 86px;
		padding: 3px;
	}
	matchstick {
		transform: rotate(90deg) translate(3px, -80px);
		transform-origin: top left;
		display: block;
		opacity: 0.1;
		width: 9px;
		height: 80px;
	}
	spot.active {
	}
	spot.active matchstick {
		opacity: 1;
	}
</style>