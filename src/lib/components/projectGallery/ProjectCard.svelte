<script>
    import { fly } from "svelte/transition";
    let { title, image, slug } = $props();

    let imgLoaded = $state(false);
    let animateTitle = $state(false);
 
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
            {#if imgLoaded && animateTitle}
                <h2 
                    class="project-title"
                    transition:fly={{ y: 100, duration: 700 }}
                >{title}
                </h2>
            {/if}
        </a>
    {#if imgLoaded}
        <div class="shadow"></div>
    {/if}
    <img 
         src={image} 
         alt={title} 
         onload={() => (imgLoaded = true)}
         class:loaded={imgLoaded}
    />
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
        opacity: 0;
        transition: all 1s ease-in-out;
        border-radius: 3rem;
        z-index: 10;
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


    @media screen and (min-width: 815px) {
       .project-card { 
            height: 340px 
        }
    }


    
    
</style>