<script>
	import { onMount, createEventDispatcher } from "svelte";

	let ready = false;
	let dispatcher = createEventDispatcher();

	function signalDemoMode() {
		dispatcher("demo");
	}

	onMount(() => {
		ready = true;
	});
</script>

{#if ready}
	<button on:click={signalDemoMode}>Use Demo Mode</button>
{/if}

<style>
	button {
		outline: none;
		background: none;
		border: 1px solid white;
		cursor: pointer;
		padding: 1px 8px;
		margin-top: 15px;
		transition: 0.2s cubic-bezier(0, 0.55, 0.45, 1);
		font-size: 12px;
        position: absolute;
        bottom: 20px;
	}

	button:hover {
		background-color: white;
		color: black;
	}
</style>
