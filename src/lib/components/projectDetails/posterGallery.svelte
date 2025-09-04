<script>
    import { fade } from "svelte/transition";
    import { innerWidth } from "svelte/reactivity/window";
    let { posters } = $props();
    let selectedIndex = $state(0);
    let imageActive= $state(false);
   let selectedImageSrc = $state(null);

    const handlePosterSelect = (index) => {
        selectedIndex = index;
    }

    const handleImageClick = (src) => {
    if (!imageActive && innerWidth.current >= 900) {
      selectedImageSrc = src;
      imageActive = true;
      document.body.style.overflow = 'hidden'
    }
  }

  const closeResizedImage = () => {
    if (imageActive) {
            imageActive = false;
            selectedImageSrc = null;
            document.body.style.overflow = 'auto'
        }    
  }

  const closeResizedImageOnEsc = (event) => {
    if (event.key == "Escape") {
       closeResizedImage();
    } 
  }

</script>

<svelte:window onkeydown={closeResizedImageOnEsc} />

<div class="carousel-container">

    <div class="selected-poster-container">
        <img 
            src={posters[selectedIndex][0]} 
            alt={posters[selectedIndex][1]}
            aria-label="Selected poster"
            class="selected-poster"
            onclick={() => handleImageClick(posters[selectedIndex][0])}
        >
    </div>
    
<div 
    class="carousel"
    role="list" 
    aria-label="Poster thumbnails"
>
    {#if posters.length > 1}
    {#each posters as poster, index}
        <div 
            class="image-container"
            tabindex="0"
            role="listitem"
            aria-label={`Poster ${index + 1}${selectedIndex === index ? ' (selected)' : ''}`}
            class:active={selectedIndex === index} 
            onclick={() => handlePosterSelect(index)}
            onkeydown={(e) => {
                if (e.key === 'Enter' || e.key === ' ') handlePosterSelect(index);
            }}     
        >
            <img 
                class="poster" 
                src={poster[0]}
                alt={poster[1]}
                transition:fade={{duration: 2000}}
            >
</div>
    {/each}
    {/if}
    </div>
    {#if imageActive}
        <div 
            role="dialog"
            aria-modal="true"
            aria-label="Expanded poster view. Press Escape to close."
            tabindex="0"
            class="overlay"
            onclick={closeResizedImage}
        >
            <div class="image-scroll-wrapper">
                <img 
                    class="selected-image" 
                    src={selectedImageSrc}
                    alt="vergrößertes Bild"
                >
            </div>
        </div>
    {/if}

</div>

<style>
    .carousel-container {
        display: flex;
        flex-direction: column;
        margin: 4rem auto;
    }

    .carousel-container img {
        display: block;
    }

    .selected-poster-container {
        max-width: 85vw;
        width: 400px;
        border-radius: 3rem;
        margin: auto;
        cursor: zoom-in;
    }

    .selected-poster{
        border-radius: 3rem;
        width: 100%;
        object-fit: contain;
    }

    .carousel {
        width: 100%;
        display: flex;
        gap: 0.7rem;
        justify-content: center;
        margin: 1rem auto;
        overflow: hidden;
        height: 5rem;
    }

   .image-container {
        width: 4rem;
        height: 4rem;
        overflow: hidden;
        border-radius: 0.5rem;
        cursor: pointer;
        transition: all ease-in-out 0.5s;
   }

   .image-container:hover, .image-container.active {
        width: 4.4rem;
        height: 4.4rem;
        overflow: hidden;
        border-radius: 0.5rem;
        border: solid 0.09rem var(--white)
   }

   .image-container img {
        object-fit: contain;
        width: 100%;
        height: auto;
        overflow: hidden;
   }

@media screen and (min-width: 900px) {
    .overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100vw;
        height: 100vh;
        background-color: rgba(37, 37, 38, 0.8);
        z-index: 1000;
        overflow-y: auto;
        display: block;
        padding: 2rem 0;
}
    .image-scroll-wrapper {
    display: flex;
    justify-content: center;
    }

    .selected-image {
    max-width: 65vw;
    height: auto;
    display: block;
    }

}
   

</style>
