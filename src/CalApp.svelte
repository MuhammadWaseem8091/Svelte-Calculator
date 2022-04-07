<script>
	import Keypad from './calculator.svelte';

	let pin;
	$: view = pin ? pin : '0';

    
	function handleSubmit() {
        alert(`submitted ${pin}`);
	}
</script>

<h1 style="color: {pin ? '#333' : '#ccc'}">{view}</h1>

<Keypad bind:value={pin} on:submit={handleSubmit}/>