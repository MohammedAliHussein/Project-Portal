<script>
    import { onMount, createEventDispatcher } from "svelte";

    let ready = false;
    let dispatcher = createEventDispatcher();

    function handleNewProject() {
        dispatcher("newProject");
    }

    onMount(() => {
        ready = true;
    });
</script>

{#if ready}
    <div class="new-project">
        <h3 on:click={handleNewProject}>New Project</h3>    
        <i class="fa-solid fa-plus" on:click={handleNewProject}></i>
    </div>
{/if}

<style>
    .new-project {
        display: flex;
        justify-content: center;
        align-items: center;
        width: fit-content;
    }

    .new-project h3 {
        margin: 10px;
    }

    h3 {
        font-size: 12px;
        cursor: pointer;
        font-weight: 500;
    }

    i {
        cursor: pointer;
    }
</style>