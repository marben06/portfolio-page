	<script>
		import ProjectArticle from "$lib/components/projectDetails/ProjectArticle.svelte";
		import TitleImage from "$lib/components/projectDetails/DemoVideo.svelte";
		import VideoPlayer from "$lib/components/projectDetails/VideoPlayer.svelte";
		import DemoVideo from "$lib/components/projectDetails/DemoVideo.svelte";
		import ImageAndtext from "$lib/components/projectDetails/ImageAndtext.svelte";
		import PosterGallery from "$lib/components/projectDetails/posterGallery.svelte";
		import AdditionalProjectArticle from "$lib/components/projectDetails/AdditionalProjectArticle.svelte";
		import ImageBoard from "$lib/components/projectDetails/ImageBoard.svelte";
		import Stack from "$lib/components/projectDetails/TechStackContainer.svelte";
		import ProjectUrlContainer from "$lib/components/projectDetails/ProjectUrlContainer.svelte";
		import TechStack from "$lib/components/projectDetails/TechStackContainer.svelte";

		let { data } = $props();
		let loadedProject = $derived(data.project);
	</script>

	{#if loadedProject}	
		{#each loadedProject.components as comp}

			{#if comp === 'article'} 
				<ProjectArticle 
					title={loadedProject.article.title} 
					content={loadedProject.article.content} 
				/>
			{/if}
			{#if comp === 'projectUrls'}
				<ProjectUrlContainer 
					urlTitle={"Test"}
					urls={loadedProject.data.projectUrls}
					github={loadedProject.data.github}
				/>
			{/if}
			{#if comp === 'demoVideo'}
				<DemoVideo 
					video={loadedProject.data.demoVideo} 
				/>
			{/if}
			{#if comp === 'videoGallery'}
				<VideoPlayer 
					videos={loadedProject.data.videos} 
				/>
			{/if}
			{#if comp === 'imgLeftTextRight'}
				<ImageAndtext 
					image={loadedProject.data.imgLeftTextRight.img} 
					text={loadedProject.data.imgLeftTextRight.text}
					altText={loadedProject.data.imgLeftTextRight.altText}
				/>
			{/if}
			
			{#if comp === 'posterGallery'}
				<PosterGallery 
					posters={loadedProject.data.posterUrls} 
				/>
			{/if}
			
			{#if comp === 'additionalArticle'}
				<AdditionalProjectArticle 
					content={loadedProject.data.additionalArticle} 
				/>
			{/if}
			
			{#if comp === 'imageBoard'}
				<ImageBoard 
					images={loadedProject.data.imageUrls} 
				/>
			{/if}
			{#if comp === 'techStack'}
				<TechStack 
					stackitems={loadedProject.data.stack} 
				/>
			{/if}
			
		{/each}
	{/if}
