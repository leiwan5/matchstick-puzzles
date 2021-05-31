<script>
	import Number from './Number.svelte';
	import Operation from './Operation.svelte';
	import Equal from './Equal.svelte';
	import Pool from './Pool.svelte';
	import min from 'lodash-es/min';
	import has from 'lodash-es/has';
	import find from 'lodash-es/find';
	import filter from 'lodash-es/filter';

	export let x = 0;
	export let y = 0;
	export let r = 0;
	export let op = '+';
	
	let updates = {};
	let fieldUpdates = {};
	
	$: data = {
		x,
		y,
		r,
		op,
		...updates
	};
	
	export let spareCapacity = 3;
	export let maxMoves = Infinity;
	let availableMoves = maxMoves;
	let spareSticks = [];
	let usedSpareSticks = 0;
	$: spareCount = spareSticks.length - usedSpareSticks;
	$: solved = spareCount === 0 && eval(`${data.x}${data.op}${data.y}`) === data.r;
	
	let version = new Date();
	
	function reset() {
		updates = {};
		availableMoves = maxMoves;
		usedSpareSticks = 0;
		spareSticks = [];
	}
	
	function onChange(target, evt) {
		const { action, stroke, index, newState } = evt.detail;
		if (action === 'off') {
			if (availableMoves === 0) return;
			spareSticks = [...spareSticks, { target, stroke, index }];
		} else if (action === 'on') {
			if (availableMoves === 0 || spareCount === 0) return;
			const stick = find(spareSticks, { target, stroke, index });
			if (stick) {
				spareSticks = filter(spareSticks, s => s !== stick);
			} else {
				usedSpareSticks++;
				availableMoves--;
			}
		}
		updates = {
			...updates,
			...(has(newState, 'value') ? { [target]: newState.value } : {}),
			newStates: {
				...updates.newStates,
				[target]: newState
			},
		}
	}
</script>

<equation key={version}>
	<Number
		state={{ value: data.x, ...data.newStates?.x }}
		readonly={availableMoves === 0}
		on:change={onChange.bind(null, 'x')}
	/>
	<Operation
		readonly={availableMoves === 0}
		state={{ value: op, ...data.newStates?.op }}
		on:change={onChange.bind(null, 'op')}
	/>
	<Number
		state={{ value: y, ...data.newStates?.y }}
		readonly={availableMoves === 0}
		on:change={onChange.bind(null, 'y')}
	/>
	<Equal />
	<Number
		state={{ value: r, ...data.newStates?.r }}
		readonly={availableMoves === 0}
		on:change={onChange.bind(null, 'r')}
	/>
</equation>

<pool>
	<Pool
		capacity={min([spareCapacity, availableMoves])}
		count={spareCount}
	/>
	<message>{availableMoves} move(s) left.</message>
</pool>
			
<result class:solved>
			{`${data.x} ${data.op} ${data.y} = ${data.r}`}
			{solved ? 'Right!' : 'Wrong!'}
</result>
			
<buttons>
	<button on:click={reset}>
		Reset
	</button>
</buttons>
			
<style>
	equation {
		display: flex;
		height: 150px;
		justify-content: center;
	}
	equation > :global(*) {
		margin: 5px;
	}
	equation :global(*) {
		user-select: none;
	}
	pool {
		display: block;
		margin: 20px auto 7px;
		padding: 10px;
		border: 1px solid #ccc;
		max-width: 400px;
		min-height: 21px;
	}
	pool message {
		display: block;
		text-align: center;
	}
	result {
		font-size: 40px;
		display: block;
		text-align: center;
		margin: 0px auto;
		color: #CC0000;
	}
	result.solved {
		color: #73d216;
	}
	buttons {
		display: block;
		margin: 10px 0;
		text-align: center;
	}
</style>
