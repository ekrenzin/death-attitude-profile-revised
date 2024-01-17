<script lang="ts">
	import type { Question } from '$lib/types';
	export let question: Question;
	export let idx: number;
	export let value: number;

	let options = [
		{ label: 'SD', selected: false },
        { label: 'D', selected: false },
        { label: 'MD', selected: false },
        { label: 'U', selected: false },
        { label: 'MA', selected: false },
        { label: 'A', selected: false },
        { label: 'SA', selected: false }
	];

	function selectOption(index: number) {
		options.forEach((option, idx) => {
			option.selected = false;
			option.selected = idx === index;
		});
		value = index + 1;
		options = [...options];
	}
</script>

<div class="question-box">
	<h3>Question {idx + 1}</h3>
	<p>{question.question}</p>
	<form>
		{#each options as option, index}
			<label>
				<input type="checkbox" checked={option.selected} on:change={() => selectOption(index)} />
				{option.label}
			</label>
		{/each}
	</form>
</div>

<style>
	input {
		cursor: pointer;
	}
	.question-box {
		padding: 0.5rem;
		background-color: rgba(0, 0, 0, 0.2);
		border-radius: 0.5rem;
		width: 100%;
		transition: 0.5s;
	}

	.question-box:hover {
		background-color: rgba(0, 0, 0, 0.1);
	}

	.question-box p {
		width: 100%;
	}
</style>
