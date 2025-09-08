<script>
    import { onMount } from "svelte";
    import ProjectCard from "./ProjectCard.svelte";

    let { loadedProjects } = $props();
</script>

<div 
    class="project-gallery"  
    role="list">
{#if loadedProjects}
    {#each loadedProjects as project}
        <ProjectCard 
            title={project.name}
            image={project.previewImage}
            slug={project.slug}
        />
    {/each}
{/if }
</div>

<style>
    .project-gallery {
        display: grid;
        gap: 3em;
        grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
        grid-template-rows: auto;
        margin: auto;
        margin-top: 0; 
        width: 90%;  
    }

    @media screen and (min-width: 1000px) {
        .project-gallery {
            grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
        }
    }
</style>


