<script>
    import ProjectGallery from "$lib/components/projectGallery/ProjectGallery.svelte";
    import ChatWidget from "$lib/components/chat/ChatWidget.svelte";
    let { data } = $props();
    let loadedProjects = $derived(data.projects)
</script>

<ProjectGallery {loadedProjects} />
<ChatWidget />
