<script>
    import { onMount } from "svelte";

    export let title = "";

    let ready = false;
    let editing = false;
    let titleEdit = "";

    function handleTitleChange() {
        editing = true;
    }

    function handleKeydown(event) {
        if(editing && event.key === "Enter") {
            //send to server
            title = titleEdit;
            editing = false;
        }
    }

    onMount(() => {
        ready = true;
    });

</script>

{#if ready}
    {#if !editing}
        <h3 on:click={handleTitleChange}>{title}</h3>
    {:else}
        <input class={`${title}-input`} placeholder={title} bind:value={titleEdit}>
    {/if}
{/if}

<svelte:window on:keydown={handleKeydown}/>

<style>
    h3 {
        font-size: 16px;
        font-weight: 700;
        margin: 10px;
        background: none;
    }

    input {
        font-size: 16px;
        font-weight: 700;
        margin: 10px 20%;
        width: 80%;
        text-align: center;
        border: none;
        outline: none;
        outline: 1px solid white;
    }
</style>