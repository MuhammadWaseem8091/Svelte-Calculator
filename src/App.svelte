<script >
	import CalApp from "./CalApp.svelte";
import Calculator from "./calculator.svelte";
import Check from "./check.svelte";

</script>

<main>
	<Calculator></Calculator>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
<!-- wassem is my bro -->