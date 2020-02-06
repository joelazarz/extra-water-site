<style>

	h1 {
		margin: 0 auto;
		text-align: center;
		font-size: 2em;
		text-transform: uppercase;
		font-weight: 700;
		margin: 0 0 0.5em 0;
	}

	.container {
		display: flex;
		flex-direction: row;
	}

	.column {
		width: 50%;
	}

	.space {
		display: flex;
		flex-direction: column;
		flex-basis: 100%;
		flex: 1;
		background-color: lightgreen;
		margin: 2%;
		}

	/* img {
		width: 100%;
		max-width: 400px;
		margin: 0 0 1em 0;
	} */

	@media (min-width: 480px) {
		h1 {
			font-size: 3em;
		}
	}

</style>

<svelte:head>
	<title>Extra Water</title>
</svelte:head>

<h1>Extra Water</h1>

<div class="container">

	<div class="column">
		<div class="space">test</div>
		<div class="space">test</div>
		<div class="space">test</div>
	</div>


	<div class="column">
		<div class="space">test</div>
		<div class="space">test</div>
		<div class="space">test</div>
	</div>

</div>





