<script>
    let { altText, imageUrl } = $props();
</script>


<div class="hero-image-container">
    <img 
        src={imageUrl}
        alt={altText}

    />
</div>

<style>
    .hero-image-container {
        overflow: hidden;
        border-radius: 30px;
        height: 30vh;
        margin-bottom: 30px;
        max-width: 750px;
        margin: auto;

        img {
            object-fit: cover;
            width: 100%;
            height: 100%;
        }
    }  

    @media screen and (min-width: 1000px) {
        .hero-image-container {
            height: 500px;
        }
    }

    

</style>