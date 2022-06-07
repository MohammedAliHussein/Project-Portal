<script>
    import { onMount } from "svelte";
    import Project from "../components/Project.svelte";
    import NewProject from "../components/NewProject.svelte";
    import axios from "axios"

    export let authToken = "demo";

    let ready = false;
    let projectData = [];

    async function handleDelete(event) {
        projectData = projectData.filter((project) => {
            return project.title !== event.detail.name
        });      
    }

    async function handleNewProject(event) {
        const project = {
            title: `Title-${projectData.length}`,
            description:"Description",
            technologies: ['Technologies'],
            github: "",
            link: "",
            hidden: false,
        }
        projectData.push(project);

        projectData = projectData;

        const response = await axios.post("http://localhost:3000/api/v1/projects", project);
    }

	onMount(async () => {
		const bulkData = (await axios.get("http://localhost:3000/api/v1/projects")).data;
		const keys = Object.keys(bulkData);
		keys.forEach((key) => {
			projectData.push(bulkData[key].project);
		});
		ready = true;
	});
</script>

{#if ready}
    <div class="portal">
        {#if authToken === "demo"}
            <h5>You are in demo mode. No changes will be submitted.</h5>
        {/if}
        <h1>Project Portal</h1>
        <NewProject on:newProject={handleNewProject}/>
        <div class="projects-section">
            {#each projectData as project}
                <Project {...project} on:deleteProject={handleDelete} authToken={authToken}/> 
            {/each}
        </div>
    </div>
{/if}

<style>
    .portal {
        display: flex;
        justify-content: center;
        align-items: center;
        flex-direction: column;
        width: 100%;
        min-height: 100vh;
    }

    .projects-section {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 250px; /*Because if we have at least 1 project, project height is 250px*/
        width: 100%;
        flex-wrap: wrap;
        overflow-y: visible;
    }

    h5 {
        font-size: 11px;
        color: rgba(255, 0, 0, 0.709);
        font-weight: 300;
        margin-bottom: 10px;
    }
</style>