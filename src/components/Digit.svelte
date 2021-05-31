<script>
	import { createEventDispatcher } from 'svelte';
	import includes from 'lodash-es/includes';
	import keys from 'lodash-es/keys';
	import filter from 'lodash-es/filter';
	import difference from 'lodash-es/difference';
	import Char from './Char.svelte';
	const strokes = ["a", "b", "c", "d", "e", "f", "g"];
	const config = {
		0: ['a', 'b', 'c', 'e', 'f', 'g'],
		1: ['c', 'f'],
		2: ['a', 'c', 'd', 'e', 'g'],
		3: ['a', 'c', 'd', 'f', 'g'],
		4: ['b', 'c', 'd', 'f'],
		5: ['a', 'b', 'd', 'f', 'g'],
		6: ['a', 'b', 'd', 'e', 'f', 'g'],
		7: ['a', 'c', 'f'],
		8: ['a', 'b', 'c', 'd', 'e', 'f', 'g'],
		9: ['a', 'b', 'c', 'd', 'f', 'g'],
	}
	const dispatch = createEventDispatcher();
	export let readonly = false;
	export let value = 0;
	export let name = undefined;
	export let nanActiveStrokes = undefined;
	export let allowNaN = true;
	
	$: activeStrokes = nanActiveStrokes ?? config[value] ?? [];
	
	function onClick(evt) {
		const removing = includes(activeStrokes, evt.detail.stroke);
		for (const digit of keys(config)) {
			const strokes = config[digit];
			const diff1 = difference(activeStrokes, strokes);
			const diff2 = difference(strokes, activeStrokes);
			if (removing) {
				if (diff2.length === 0 && diff1.length === 1 && diff1[0] == evt.detail.stroke) {
					const newValue = +digit;
					dispatch('click', { stroke: evt.detail.stroke, index: 0, action: 'off', newValue });
					return;
				}
			} else {
				if (diff1.length === 0 && diff2.length === 1 && diff2[0] == evt.detail.stroke)	{
					const newValue = +digit;
					dispatch('click', { stroke: evt.detail.stroke, index: 0, action: 'on', newValue });
					return;
				}
			}
		}
		if (allowNaN) {
			const newActiveStrokes = removing
				? filter(activeStrokes, s => s !== evt.detail.stroke)
				: [...activeStrokes, evt.detail.stroke]
			const newValue = NaN;
			dispatch('click', { stroke: evt.detail.stroke, index: 0, action: removing ? 'off' : 'on', newValue, newActiveStrokes });
		}
		
	}
</script>

<Char
	{strokes}
	{activeStrokes}
	{readonly}
	{name}
	on:click={onClick}
/>
