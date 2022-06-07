<script>
	import ProjectPortal from "./sections/ProjectPortal.svelte";
	import ProjectPortalSignin from "./sections/ProjectPortalSignin.svelte";

	import { onMount } from "svelte";

	let authenticated;
	let authToken;

	let ready = false;

	async function handleLogin(event) {
		authToken = event.detail.authToken;
		authenticated = true;
	}

	function handleLogout() {
		authenticated = false;
	}

	onMount(() => {
		if(localStorage.getItem("authToken")) {
			authToken = localStorage.getItem("authToken");
			authenticated = true;
		}
		ready = true;
	});

</script>

<main>
	{#if ready}
		{#if authenticated}
			<ProjectPortal authToken={authToken} on:logout={handleLogout}/>
		{:else}
			<ProjectPortalSignin on:login={handleLogin}/>
		{/if}
	{/if}
</main>