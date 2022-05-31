<script>
    import { onMount } from "svelte";

    let ready = false;

    onMount(() => {
        ready = true;
    });

</script>

{#if ready}
    <input type="password">
{/if}

<style>
    input {
        outline: none;
        border: none;
    }
</style>