<script>
import ExtraWaterRadio from '../components/index_components/ExtraWaterRadio.svelte'
import LogoTshirt from '../components/index_components/LogoTshirt.svelte'
</script>

<style>

	h1 {
		margin: 0 auto;
		text-align: center;
		font-size: 3em;
		text-transform: uppercase;
		font-weight: 700;
		margin: 0 0 0.5em 0;
	}

	.container {
		display: flex;
		flex-direction: column;
	}

	.column {
		width: 100%;
	}

	@media only screen and (min-width: 768px) {
		h1 {
		margin: 0 auto;
		text-align: center;
		font-size: 4em;
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

	}

</style>


<!-- html  -->

<svelte:head>
	<title>Extra Water</title>
</svelte:head>

<h1>Extra Water</h1>

<div class="container">

	<div class="column">
		<LogoTshirt />
		<ExtraWaterRadio />
		<ExtraWaterRadio />
	</div>


	<div class="column">
		<ExtraWaterRadio />
		<ExtraWaterRadio />
		<ExtraWaterRadio />
	</div>

</div>





