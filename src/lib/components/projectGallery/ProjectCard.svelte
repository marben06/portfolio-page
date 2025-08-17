<script>
    import { fly } from "svelte/transition";

    let { title, image, slug } = $props();

    // testing enhanced:image atm, if kept path will be adjustet directly in source json
    const imageModules = import.meta.glob('../../assets/projects-preview/*.{webp,jpg,png}', { eager: true, query: { enhanced: true } });
     // Extract the filename from the image path
    const filename = image.split('/').pop();
    const relativeImagePath = `../../assets/projects-preview/${filename}`;
    const mod = imageModules[relativeImagePath];

    if (!mod) {
        console.error(
        `[ProjectCard] Missing image module for: ${relativeImagePath}.` +
        ` Available keys: ${Object.keys(imageModules).join(', ')}`
        );
    }

  const projectImage = mod?.default;
    
    let animateTitle = $state(false);
    let imgLoaded = $derived(projectImage ? true : false)

</script>

<div 
    class="project-card"
    role="listitem"
>
        <a 
            href="/projects/{slug}" 
            class="project-link"
            onfocus={() => (animateTitle = true)}
            onblur={() => (animateTitle = false)}
            onpointerenter={() => (animateTitle = true)}   
            onpointerleave={() => (animateTitle = false)}
        >        
            {#if animateTitle && (imgLoaded || !mod)}
                <h2 
                    class="project-title"
                    transition:fly={{ y: 100, duration: 700 }}
                >{title}
                </h2>
            {/if}
        </a>
        <div class="shadow">
            {#if projectImage}
                <enhanced:img 
                    src={projectImage} 
                    alt={title} 
                    onloadeddata={() => (imgLoaded = true)}
                    class:loaded={imgLoaded}
                />
            {:else}
                <div class="image-error"><span>Bild konnte nicht geladen werden.</span></div>
            {/if}
            

    </div>
</div>



<style>

    .shadow {
        width: 100%;
        height: 100%;
        z-index: 11;
        box-shadow: rgba(50, 50, 93, 0.25) 0px 30px 60px -12px inset, rgba(0, 0, 0, 0.3) 0px 18px 36px -18px inset;
    }

    .project-card {
        display: flex;
        height: 380px;
        overflow: hidden;
        border-radius: 3rem;
        position: relative;
    }

    img {
        object-fit: cover;
        width: 100%;
        height: 100%;
        position: absolute;
        transition: all 1s ease-in-out;
        border-radius: 3rem;
        z-index: 10;
        opacity: 0;
    }

    .loaded {
        opacity: 1;
    }

    .project-link {
        position: absolute;
        background-color: var(--accent);
        opacity: 0;
        z-index: 100;
        width: 100%;
        height: 100%;
        cursor: pointer;
        color: var(--white);
        display: flex;
        align-items: center;
        justify-content: center;
        transition: opacity 0.8s; 
        text-align: center;
        border-radius: 3rem;
    }

    .project-link:hover, .project-link:focus {
        opacity: 0.9;

    }

    .project-title {
        padding: 0.4rem;
        font-size: 1.1rem;
    }

     .project-title:focus {
         opacity: 1;
    }

    .image-error {
        height: 100%;
        width: 100%;
        background-color: var(--accent);
        text-align: center;
    }

    .image-error span {
        font-size: 0.8rem;
    }


    @media screen and (min-width: 815px) {
       .project-card { 
            height: 340px 
        }
    }


    
    
</style>