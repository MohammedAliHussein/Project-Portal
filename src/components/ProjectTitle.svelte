<script>
    import { onMount } from "svelte";

    export let title = "";

    let ready = false;
    let editing = false;
    let vis = "display: none";

    function handleTitleChange() {
        editing = true;
        vis = "";
    }

    onMount(() => {
        ready = true;
    });

</script>

{#if ready}
    {#if !editing}
        <h3 on:click={handleTitleChange}>{title}</h3>
    {/if}
    <input class={`${title}-input`} style={vis} placeholder={title}>
{/if}

<style>
    h3 {
        font-size: 16px;
        font-weight: 700;
        margin: 10px;
        background: none;
    }

    input {
        z-index: 1000;
        font-size: 16px;
        font-weight: 700;
        margin: 10px 20%;
        width: 80%;
        text-align: center;
        border: none;
        outline: none;
    }
</style>