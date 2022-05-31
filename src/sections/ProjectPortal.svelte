<script>
    import { onMount } from "svelte";
    import Project from "../components/Project.svelte";
    import NewProject from "../components/NewProject.svelte";
    import axios from "axios"

    export let authToken = null;

    let ready = false;
    let projectData = [];

    function handleDelete(event) {
        projectData = projectData.filter((project) => {
            return project.title !== event.detail.name
        });      
    }

    function handleNewProject(event) {
        projectData.push({
            title: "Title",
            description:"Description",
            technologies: ['Technologies'],
            github: "",
            link: "",
            hidden: false,
        });

        projectData = projectData;
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
        <h1>Project Portal</h1>
        <NewProject on:newProject={handleNewProject}/>
        <div class="projects-section">
            {#each projectData as project}
                <Project {...project} on:deleteProject={handleDelete}/> 
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
</style>