<script>
    import { onMount } from "svelte";
    import { page } from '$app/stores'; 
    import typewriter from "../../utils/typewriterTransition";
    import { fade } from "svelte/transition";
    import { useVisibilityObserver } from "../../utils/useVisibilityObserver.svelte";
	import { render } from "svelte/server";
    import { innerHeight, innerWidth } from "svelte/reactivity/window";
    import { afterNavigate } from "$app/navigation";
    import { scrollY } from "svelte/reactivity/window";

    let elementToObserve = $state(null);
    let observer = $state(null);
    let renderTimeout = 4000;

    let renderNav = $state(false);
    let base = $derived($page.url.pathname === "/" ? true : false) 
    
    let titleHeight = $state();
    let pageHeight = $state(0);
	let pageHeightVh =  $state(10);
    let hasScrollRoom = $state(false);


    const navButtons = [
            {url: '/projects', name: 'Projekte'},
            {url: '/about', name: 'About'},
            {url: '/contact', name: 'Kontakt'}
        ];

    onMount(() => {
    
        calculateHeights();

        observer = useVisibilityObserver(elementToObserve);

        $effect(() => {
            if (observer) {
                setTimeout(() => {
                    renderNav = true;
                }, renderTimeout)
            }});
    });

    afterNavigate(() => {
        calculateHeights();
    });

    const calculateHeights = () => {
        pageHeight = document.documentElement.scrollHeight;
        pageHeightVh = (pageHeight / innerHeight.current) * 100;
    };

</script>

<header bind:this={elementToObserve}>
    <div class="title-container"
    >
        {#if (observer && observer.isVisible) || !base || innerWidth.current <= 600}
            <h1 
                id="title"
                bind:clientHeight={titleHeight}
                transition:typewriter
            >
                Benedikt Martini | Informationsdesign
            </h1>
        {/if}
    </div>

    <!-- only create fixed navigation when it is not base page and when the page has more height than viewport -->
    <nav 
        class="navigation-bar"
        class:fixed={scrollY.current >= titleHeight && !base && pageHeightVh >= 120} 
    >
        {#each navButtons as button, index}
            <a 
                class="nav-link"
                class:active={$page.url.pathname.slice(0, -1) === button.url} 
                href={button.url}
            >{button.name}
            </a>
        {/each}
    </nav>
</header>   

<style>
    #title {
        font-size: 1.5rem;
        text-align: center;
        margin: 1rem auto;
        align-items: center;
        justify-content: center;
    }

    .title-container {
        min-height: 7rem;
        height: fit-content;
        display: flex;
        margin-top: 1rem;
    }

    .navigation-bar {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
        margin: auto;
        margin-bottom: 4rem;
        z-index: 500;
        height: 4rem;
        justify-content: space-evenly;
    }

    .fixed {
        position: fixed;
        top: 0;
        left: 50%; 
        background-color: var(--background);
        transform: translateX(-50%);
        width: 100%;
    }

    .nav-link {          
        margin: 0.1rem;
        padding: 10px;
        border: 3px solid var(--white);
        width:fit-content;
        transition: all 0.3s;
    }

    .nav-link:hover, .nav-link.active {          
        background-color: var(--white);
        color: var(--accent)
    }

      @media screen and (min-width: 600px) {
        .navigation-bar {
            min-width: 550px;
            width: 100%;
        }

        .title-container {
            min-height: 5rem;
        }

  
    }

</style>