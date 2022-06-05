<script>
    import { onMount } from "svelte";
    import axios from "axios";

    export let description = "";
    export let title = "";

    $: descriptionEdit = description;

    let ready = false;
    let editing = false;

    function handleKeyDown(event) {
        if(event.key === "Escape") {
            descriptionEdit = description;
            editing = false
        } else {
            if(event.key === "Enter") {
                if(descriptionEdit.length > 0) {
                    description = descriptionEdit;
                    axios.put("http://localhost:3000/api/v1/projects/updateDescription", {
                        title,
                        description
                    })
                } else {
                    description = "Description";
                }
                editing = false;
            }
        }
    }

    function handleDescriptionChange() {
        editing = true;
    }

    onMount(() => {
        ready = true;
    });
</script>

{#if ready}
    <div class="description">
        {#if !editing}
            <h3 on:click={handleDescriptionChange}>{description}</h3>
        {:else}
            <textarea class="input" cols="50" rows="4" bind:value={descriptionEdit}></textarea>
        {/if}
    </div>
{/if}

<svelte:window on:keydown={handleKeyDown}/>

<style>
    .description {
        text-align: center;
        width: 80%;
        background: none;
    }

    h3 {
        font-size: 12px;
        font-weight: 300;
        color: rgb(210, 210, 210);
        line-height: 20px;
        background: none;
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