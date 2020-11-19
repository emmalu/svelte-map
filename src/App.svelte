<script>
	import Tailwindcss from './Tailwindcss.svelte';
	import MapView from './MapView.svelte';

	export let name;

	$: document.title = name;

</script>

<Tailwindcss />
<main>
	<h1>hello {name}!</h1>
	<p>This is a sample map app developed by <a href="https://emmalu.github.io/portfolio" class="font-bold">MapMazeLu</a> at <span class="font-semibold">Adonde GeoSpatial</span>.</p>
	<MapView />
</main>

<style>
	main {
		text-align: center;
		padding: .5em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 3em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
