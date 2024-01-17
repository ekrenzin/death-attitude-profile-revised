<script>
	import { questions } from './questions';
	import QuestionRow from '$lib/Question.svelte';

	let allQuestions = questions.map((question, idx) => ({ question, value: null, idx }));
	let results = null;

	function submit() {
		const data = allQuestions.map(({ question, value }) => ({ question, value }));

		// Check for missing values
		if (data.some(({ value }) => value === null)) {
			alert('Please answer all questions');
			return;
		}

		// Initialize an object to hold category scores
		let categoryScores = {};

		// Add up scores for each category
		data.forEach(({ question, value }) => {
			let category = question.category; // Assuming each question has a 'category' property
			if (!categoryScores[category]) {
				categoryScores[category] = { total: 0, count: 0 };
			}
			categoryScores[category].total += value;
			categoryScores[category].count++;
		});

		// Calculate average score for each category
		let scores = {};
		for (let category in categoryScores) {
			scores[category] = categoryScores[category].total / categoryScores[category].count;
		}

		results = scores;
	}
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section>
	<h1>Death Attitude Profile-Revised (DAP-R)</h1>
	<h2>Wong, P.T.P., Reker, G.T., & Gesser, G.</h2>
	<p>
		This questionnaire contains a number of statements related to different attitudes toward death.
		Read each statement carefully, and then decide the extent to which you agree or disagree. For
		example, an item might read: “Death is a friend.” Indicate how well you agree or disagree by
		circling one of the following: SA = strongly agree; A= agree; MA= moderately agree; U=
		undecided; MD= moderately disagree; D=disagree; SD= strongly disagree. Note that the scales run
		both from strongly agree to strongly disagree and from strongly disagree to strongly agree. If
		you strongly agreed with the statement, you would circle SA. If you strongly disagreed you would
		circle SD. If you are undecided, circle U. However, try to use the undecided category sparingly.
		It is important that you work through the statements and answer each one. Many of the statements
		will seem alike, but all are necessary to show slight differences in attitudes.
	</p>
	<div class="questions">
		{#each allQuestions as question, idx}
			<QuestionRow question={question.question} {idx} bind:value={question.value} />
		{/each}
	</div>
	<button on:click={submit}>Submit</button>
	{#if results}
		<h1>Results:</h1>
		{#each Object.entries(results) as [category, score]}
			<h2>{category}: {score}</h2>
		{/each}
	{/if}
	<p>
		For further information on the theoretical rationale and the psychometric properties of the
		scale consult the following source: Wong, P.T.P., Reker, G.T., & Gesser, G. (1994). Death
		Attitude Profile-Revised: A multidimensional measure of attitudes toward death. In R.A. Neimeyer
		(Ed.), Death anxiety handbook: Research, instrumentation, and application. (pp. 121-148).
		Washington, DC: Taylor & Francis. For information on the original DAP, consult the following
		source: Gesser, G., Wong, PT.P., & Reker, G.T. (1987-88). Death attitudes across the life span:
		The development and validation of the Death Attitude Profile (DAP). Omega, 18, 113-128.
	</p>
</section>

<style>
	.questions {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
		width: 600px;
		max-width: 80%;
		gap: 1rem;
	}

	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	h1 {
		width: 100%;
		text-align: center;
	}
</style>
