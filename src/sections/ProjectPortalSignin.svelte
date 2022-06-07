<script>
    import InputField from "../components/InputField.svelte";
    import SubmitButton from "../components/SubmitButton.svelte";
    import Title from "../components/Title.svelte";
    import DemoModeButton from "../components/DemoModeButton.svelte";
    import axios from "axios";

    import { onMount, createEventDispatcher } from "svelte";

    let ready = false;
    let dispatcher = createEventDispatcher();
    let loginFailed = false;
    let errorMessage = "";

    function handleFailedLogin() {
        loginFailed = true;
    }

    async function handleSignin() {
        const username = document.querySelector(".email").value;
        const password = document.querySelector(".password").value;

        const auth = await axios.post("http://localhost:3000/portal/login", { 
            username: username ? username : "", 
            password: password ? password : "" 
        }).catch((error) => {
            console.log(error);
            if(error.response.status === 401) {
                errorMessage = error.response.data;            
                handleFailedLogin();
            }
        });

        if(auth.status === 200) {
            localStorage.setItem("authToken", auth.data.authToken);
            dispatcher("login", { authToken: auth.data.authToken });
        } 
    }

    function handleDemoMode() {
        dispatcher("login", { auth: "demo" });
    }

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
        {#if loginFailed}
            <h5>{errorMessage}</h5>
        {/if}
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

    h5 {
        margin-top: 10px;
        font-size: 11px;
        font-weight: 400;
    }
</style>