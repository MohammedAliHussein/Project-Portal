<script>
    import { onMount, createEventDispatcher } from "svelte";

    let ready = false;
    let dispatcher = createEventDispatcher();

    function handleLogout() {
        if(localStorage.getItem("authToken")) {
            localStorage.removeItem("authToken");
        }        
        dispatcher("logout");
    }

    onMount(() => {
        ready = true;
    })
</script>

{#if ready}
    <button on:click={handleLogout}>Logout</button>
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
        margin-bottom: 5px;
	}

	button:hover {
		background-color: white;
		color: black;
	}
</style>