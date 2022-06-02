<script>
    import { onMount } from "svelte";

    export let technologies = [];

    let ready = false;
    let editing = false;
    let editedTechnologies = technologies;

    function handleChangeTechnology() {
        editing = true;
    }

    function handleKeyDown(event) {
        if(event.key === "Escape") {
            editedTechnologies = technologies;
            editing = false;
        } else {
            if(event.key === "Enter" && editing) {
                //send to server
                technologies = editedTechnologies.split(",");
                editing = false;
            }
        }
    }

    onMount(() => {
        ready = true;
    });
</script>

{#if ready}
    <div class="technologies">
        {#if !editing}
            {#each technologies as technology}
                <h4 on:click={handleChangeTechnology}>{technology}</h4>
            {/each}
        {:else}
            <textarea class="input" name="" cols="30" rows="1" bind:value={editedTechnologies} placeholder={technologies}></textarea>
        {/if}
    </div>
{/if}

<svelte:window on:keydown={handleKeyDown}/>

<style>
    .technologies {
        width: 75%;
        height: fit-content;
        display: flex;
        flex-wrap: wrap;
        height: fit-content;
        justify-content: space-evenly;
        align-items: center;
        background: none;
    }

    h4 {
        background: none;
        font-weight: 400;
        font-size: 12px;
        line-height: 25px;
        margin-right: 10px;
    }

    .input {
        font-size: 12px;
        font-weight: 300;
        color: rgb(210, 210, 210);
        line-height: 20px;
        text-align: center;
        border: none;
        outline: none;
        height: fit-content;
    }
</style>