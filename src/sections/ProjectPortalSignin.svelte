<script>
    import InputField from "../components/InputField.svelte";
    import SubmitButton from "../components/SubmitButton.svelte";
    import Title from "../components/Title.svelte";
    import DemoModeButton from "../components/DemoModeButton.svelte";
    import axios from "axios";

    import { onMount, createEventDispatcher } from "svelte";

    let ready = false;
    let dispatcher = createEventDispatcher();

    async function handleSignin() {
        const username = document.querySelector(".email").value;
        const password = document.querySelector(".password").value;

        // const auth = await axios.post("http://localhost:3000/portal/login", { username, password });

        const auth = {
            status: 200
        }

        auth.status === 200 ? dispatcher("login", { jwt: "dummy" }) : ''
    }

    function handleDemoMode(){}

    onMount(() => {
        ready = true;
    });
</script>

{#if ready}
    <div class="project-portal-signin">
        <Title title={"Project Portal"}/>
        <InputField fieldType={"email"} fieldName={"Username"} animationDelay={200}/>
        <InputField fieldType={"password"} fieldName={"Password"} animationDelay={400}/>
        <SubmitButton on:signin={handleSignin} animationDelay={600}/>
        <DemoModeButton on:demo={handleDemoMode}/>
    </div>
{/if}

<style>
    .project-portal-signin {
        width: 100%;
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
    }
</style>