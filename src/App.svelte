<script>
	import ProjectPortal from "./sections/ProjectPortal.svelte";
	import ProjectPortalSignin from "./sections/ProjectPortalSignin.svelte";

	import { onMount } from "svelte";

	let authenticated;
	let authToken;

	let ready = false;

	function handleLogin(event) {
		authToken = event.detail.jwt;
		authenticated = true;
	}

	onMount(() => {
		ready = true;
	});

</script>

<main>
	{#if ready}
		{#if authenticated}
			<ProjectPortal authToken={authToken}/>
		{:else}
			<ProjectPortalSignin on:login={handleLogin}/>
		{/if}
	{/if}
</main>