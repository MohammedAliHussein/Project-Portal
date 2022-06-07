<script>
    import ProjectTitle from "./ProjectTitle.svelte";
    import ProjectDescription from "./ProjectDescription.svelte";
    import ProjectTechnologies from "./ProjectTechnologies.svelte";

    import { fly } from "svelte/transition";
    import { onMount, createEventDispatcher } from "svelte";
    import axios from "axios";

    export let title = "title";
    export let description = "description";
    export let technologies = [];
    export let github = "";
    export let link = "";
    export let inProgress = false;
    export let index = 0;
    export let authToken = "demo";

    let ready = false;
    let linkEditing = false;
    let githubEditing = false;
    let editedGithub = github;
    let editedLink = link;
    let dispatcher = createEventDispatcher();

    function handleLinkChange() {
        if(githubEditing) {
            githubEditing = false;
        }
        linkEditing = true;
    }

    function handleGithubChange() {
        if(linkEditing) {
            linkEditing = false;
        }
        githubEditing = true;
    }

    async function handleDelete() {
        dispatcher("deleteProject", {
            name: title
        });

        if(authToken !== "demo") {
            const response = await axios.put("http://localhost:3000/api/v1/projects/deleteProject", {
                data: title, 
                headers: {
                    "authToken": localStorage.getItem("authToken")
                }
            });
        }
    }

    function handleKeyDown(event) {
        if(event.key === "Escape") {
            resetGithubEdit();
            resetLinkEdit();
        } else {
            handleGithubLinkConfirm(event.key);
            handleLinkConfirm(event.key);
        }
    }

    async function handleGithubLinkConfirm(key) {
        if(githubEditing && key === "Enter") {
            github = editedGithub;
            if(authToken !== "demo") {
                const response = await axios.put("http://localhost:3000/api/v1/projects/updateGithub", {
                    data: {
                        title: title,
                        github: github
                    },
                    headers: {
                        "authToken": localStorage.getItem("authToken")
                    }                    
                });
            }

            githubEditing = false;
        }
    }

    async function handleLinkConfirm(key) {
        if(linkEditing && key === "Enter") {
            link = editedLink;
            if(authToken !== "demo") {
                const response = await axios.put("http://localhost:3000/api/v1/projects/updateLink", {
                    data: {
                        title,
                        link
                    },
                    headers: {
                        "authToken": localStorage.getItem("authToken")
                    }                    
                });
            }
            linkEditing = false;
        }
    }

    async function resetGithubEdit() {
        githubEditing = false;
        editedGithub = github;
    }

    async function resetLinkEdit() {
        linkEditing = false;
        editedLink = link;
    }

    async function setInProgress() {
        inProgress = !inProgress;
        if(authToken !== "demo") {
            const response = await axios.put("http://localhost:3000/api/v1/projects/updateProgress", {
                data: {
                    title,
                    progress: inProgress
                },
                headers: {
                    "authToken": localStorage.getItem("authToken")
                }
            });
        }
    }

    onMount(() => {
        ready = true;
    });
</script>

{#if ready}
    <div class="project" in:fly={{duration: 500, y: -20, delay: (index * 300) + 300}} out:fly={{duration: 300, opacity: 0}} >
        <i id="delete" class="fa-solid fa-xmark" on:click={handleDelete}></i>
        <ProjectTitle title={title} authToken={authToken}/>
        <div class="links">
            {#if github.length > 0}
                {#if !githubEditing}
                    <i class="fa-brands fa-github fa-lg" on:click={handleGithubChange}></i>
                {:else}
                    <input type="text" placeholder={github} bind:value={editedGithub}>
                {/if}
            {:else}
                {#if !githubEditing}
                    <i id="hidden" class="fa-brands fa-github fa-lg" on:click={handleGithubChange}></i>
                {:else}
                    <input type="text" placeholder={"Github link"} bind:value={editedGithub}>
                {/if}
            {/if}
            {#if link.length > 0}
                {#if !linkEditing}
                    <i class="fa-solid fa-up-right-from-square fa-lg" on:click={handleLinkChange}></i>    
                {:else}
                    <input type="text" placeholder={link} bind:value={editedLink}>
                {/if}
            {:else}
                {#if !linkEditing}
                    <i id="hidden" class="fa-solid fa-up-right-from-square fa-lg" on:click={handleLinkChange}></i>  
                {:else}
                    <input type="text" placeholder={"Link"} bind:value={editedLink}>
                {/if}
            {/if}
            {#if inProgress}
                <i class="fa-solid fa-hammer fa-fade" on:click={setInProgress}></i>
            {:else}
                <i class="fa-solid fa-hammer"  style="opacity: 0.5;" on:click={setInProgress}></i>
            {/if}
        </div>
        <ProjectDescription title={title} description={description} authToken={authToken}/>
        <ProjectTechnologies title={title} technologies={technologies} authToken={authToken}/>
    </div>
{/if}

<svelte:window on:keydown={handleKeyDown}/>

<style>
    .project {
        display: flex;
        justify-content: space-evenly;
        align-items: center;
        flex-direction: column;
        width: 400px;
        height: 250px;
        filter: drop-shadow(0px 0px 8px rgba(0, 0, 0, 0.25));
        margin: 20px;
        outline: 1px solid rgba(72, 72, 72, 0.1);
        position: relative;
       
    }

    @keyframes slideIn {
        0% {
            opacity: 0;
            transform: translateY(-20px);
        }
        100% {
            opacity: 1;
            transform: translateY(0px);
        }
    } 

    .links {
        height: fit-content;
        width: 100%;
        display: flex;
        justify-content: space-evenly;
        align-items: center;
        background: none;
    }

    i {
        cursor: pointer;
    }

    #delete {
        position: absolute;
        top: 0;
        right: 0;
        margin: 10px;
    }

    #hidden {
        color: rgba(255, 255, 255, 0.4);
    }

    input {
        font-size: 12px;
        font-weight: 400;
        text-align: center;
        border: none;
        outline: none;
        margin: 0;
    }
</style>