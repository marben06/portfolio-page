<script>
    import { onMount } from "svelte";
    let { video } = $props();

</script>


<div class="video-player">
    <video 
        class="video" 
        src={video}
        controls
        >Dies Video kann in Ihrem Browser nicht wiedergegeben werden.
        <track kind="captions">
    </video>
</div>

<style>
    .video-player {
        height: fit-content;
        margin: 40px auto;
        max-width: 780px;
        border-radius: 20px;
    }

    .video {
        width: 100%;
    }

</style>