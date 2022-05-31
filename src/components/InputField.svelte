<script>
    import { onMount, createEventDispatcher } from "svelte";
    import { fly } from "svelte/transition";
    import { circOut } from "svelte/easing";

    export let fieldType = "text";
    export let fieldName = "field";
    export let animationDelay = 0;

    let ready = false;

    onMount(() => {
        ready = true;
    });

</script>

{#if ready}
    <input class={fieldType} in:fly={{ duration: 300, y: -20, delay: animationDelay, easing: circOut}} type={fieldType} placeholder={fieldName}>
{/if}

<style>
    input {
        outline: none;
        border: 1px solid rgba(255, 255, 255, 0.091);
        text-align: center;
        margin-top: 10px;
        padding: 3px 13px;
        transition: 0.2s cubic-bezier(0, 0.55, 0.45, 1);
        font-size: 12px;
    }

    input:focus {
        border: 1px solid rgba(255, 255, 255, 0.25);
    }
</style>