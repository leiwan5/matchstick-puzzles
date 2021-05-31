<script>
	import { createEventDispatcher } from 'svelte';
	import Digit from './Digit.svelte';
	import sum from 'lodash-es/sum';
	export let state = { value: 0 };
	export let readonly = false;
	$: digits = state.value.toString(10).split('').map(d => +d);
	$: length = digits.length;

	const dispatch = createEventDispatcher();
	
	function onClick({ index, stroke, action, newValue: newDigitValue, newActiveStrokes }) {
		if (isNaN(newDigitValue)) {
			const newState = {
				...state,
				nanDigits: {
					...state.nanDigits,
					[index]: newActiveStrokes,
				}
			}
			dispatch('change', { stroke, index, action, newState });
		} else {
			if (length > 1 && index === 0 && newDigitValue === 0) return;
			const newValue = sum(digits.map((d, idx) => {
				return (idx === index ? newDigitValue : d) * Math.pow(10, (length - 1 - idx))
			}));
			const newState = {
				...state,
				value: newValue,
				nanDigits: {
					...state.nanDigits,
					[index]: null,
				}
			}
			dispatch('change', { stroke, index, action, newState });
		}
	}
</script>

{#each digits as digit, idx}
	<Digit
		value={digit}
		nanActiveStrokes={state.nanDigits?.[idx]}
		{readonly}
		on:click={(evt) => onClick({ ...evt.detail, index: idx })}
	/>
{/each}