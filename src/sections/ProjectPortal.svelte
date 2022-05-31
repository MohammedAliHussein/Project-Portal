<script>
    import { onMount } from "svelte";
    import axios from "axios"

    export let authToken = null;

    let ready = false;
    let projectData = [];

	onMount(() => {
		// const bulkData = (await axios.get("http://localhost:3000/api/v1/projects")).data;

		// const keys = Object.keys(bulkData);

		// keys.forEach((key) => {
		// 	projectData.push(bulkData[key].project);
		// });

		ready = true;
	});
</script>

{#if ready}
    <div class="portal">
        <h1>{authToken}</h1>
    </div>
{/if}

<style>
    .portal {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 100vh;
    }
</style>