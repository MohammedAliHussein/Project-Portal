<script>
    import { onDestroy, onMount } from "svelte";
    import axios from "axios";

    export let title = "";
    export let authToken = "demo";

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
                    if(authToken !== "demo") {
                        const response = await axios.put("https://localhost:443/api/v1/projects/updateTitle", { 
                            data: {
                                title,
                                newTitle: titleEdit, 
                            },
                            headers: {
                                "authToken": localStorage.getItem("authToken")
                            }
                        });
                        //handleErrorResponse
                    }
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