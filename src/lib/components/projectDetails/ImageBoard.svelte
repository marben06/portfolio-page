<script>
  import { innerWidth } from "svelte/reactivity/window";
  let { images } = $props(); 
  //let selectedImageSrc = $state("/images/projects/diskurs_bierdeckel_1.jpg");
  let imageActive= $state(false);
  let selectedImageSrc = $state(null);

  const TOTAL_COLUMNS = 12;
  const maxSpan = 6;
  const minSpan = 4;


  // Assign random layout metadata (start and span)
  images = images.map((image) => {
    const span = Math.floor(Math.random() * (maxSpan - minSpan + 1)) + minSpan;
    const maxStart = TOTAL_COLUMNS - span + 1;
    const start = Math.floor(Math.random() * maxStart) + 1;
    return {
      url: image[0],
      alt: image[1],
      gridColumn: `${start} / span ${span}`
    };
  });

  const handleImageClick = (index) => {
    if (!imageActive && innerWidth.current >= 900) {
      selectedImageSrc = images[index].url;
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

<div class="image-board">
  {#each images as image, index}
    <img 
      src={image.url} 
      alt={image.alt} 
      style={`grid-column: ${image.gridColumn}`}
      onclick={() => handleImageClick(index)}
      
    />
  {/each}

  {#if imageActive}
        <div 
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
  .image-board {
    margin: 4rem auto;
    width: 100%;
    position: relative;
  }

  img {
    width: 100%;
    margin: 1rem auto;
    max-width: 100%;
    border-radius: 0.5rem;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    box-sizing: border-box;
    cursor: zoom-in;
  }

  img:hover {
    opacity: 0.7;
  }

  @media screen and (min-width: 600px) {
    .image-board {
        display: grid;
        grid-template-columns: repeat(12, 1fr);
        gap: 2rem;
    }

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
