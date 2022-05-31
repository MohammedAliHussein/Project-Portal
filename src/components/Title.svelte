<script>
	export let title = "title";

	import { onMount } from "svelte";

	let ready = false;

	onMount(() => {
		ready = true;
	});
</script>

{#if ready}
	<h2>{title}</h2>
{/if}

<style>
	h2 {
		font-size: 32px;
		font-weight: 700;
		animation: slideIn 0.5s cubic-bezier(0, 0.55, 0.45, 1);
	}

	@keyframes slideIn {
		0% {
			opacity: 0;
			transform: translateY(-20px);
		}
		100% {
			opacity: 1;
			transform: translateY(0px);
		}
	}
</style>
