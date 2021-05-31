<script>
	import { createEventDispatcher } from 'svelte';
	import Char from './Char.svelte';
	export let readonly = false;
	export let state = { value: '+' };
	
	const dispatch = createEventDispatcher();

	function onChange({ stroke }) {
		if (stroke === 'h') {
			dispatch('change', {
				stroke,
				action: (state.value === '+') ? 'off' : 'on',
				newState: { value: (state.value === '+') ? '-' : '+' },
			});
		}
	}
</script>

<Char
	strokes={["d", "h"]}
	activeStrokes={state.value === '+' ? ['d', 'h'] : ['d']}
	lockedStrokes={["d"]}
	{readonly}
	on:click={evt => onChange(evt.detail)}
/>
