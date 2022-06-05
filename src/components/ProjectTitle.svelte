<script>
    import { onDestroy, onMount } from "svelte";
    import axios from "axios";

    export let title = "";

    let ready = false;
    let editing = false;
    let titleEdit = "";

    function handleTitleChange() {
        editing = true;
    }

    async function handleKeydown(event) {
        if(event.key === "Escape") {
            editing = false;
            titleEdit = title;
        } else {
            if(editing && event.key === "Enter") {
                if(titleEdit.length > 0) {
                    const response = await axios.put("http://localhost:3000/api/v1/projects/updateTitle", { 
                        title,
                        newTitle: titleEdit, 
                    });
                    //handleErrorResponse
                    title = titleEdit;
                } else {
                    title = title;
                }
                editing = false;
            }
        }
    }

    onMount(() => {
        ready = true;
    });

    onDestroy(() => {
        editing = false;
    });
</script>

{#if ready}
    {#if !editing}
        <h3 on:click={handleTitleChange}>{title}</h3>
    {:else}
        <input placeholder={title} bind:value={titleEdit}>
    {/if}
{/if}

<svelte:window on:keydown={handleKeydown}/>

<style>
    h3 {
        font-size: 16px;
        font-weight: 700;
        margin: 10px;
        background: none;
        cursor: pointer;
    }

    input {
        font-size: 16px;
        font-weight: 700;
        margin: 10px 20%;
        width: 80%;
        text-align: center;
        border: none;
        outline: none;
    }
</style>