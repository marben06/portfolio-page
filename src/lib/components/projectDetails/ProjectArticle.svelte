<script>
import { onMount } from "svelte";
import { useVisibilityObserver } from "../../../utils/useVisibilityObserver.svelte";
import { slide } from "svelte/transition";

let { title, content  } = $props();

let elementToObserve = $state(null);
let observer = $state(null);
let renderElements = $state(false);

</script>

<div class="render-container">
    <article class="project-description">
        <h2>{title}</h2>
        <div>{@html content}</div>
    </article>
</div>

<style>

    h2 {
        font-size: 1.8rem;
    }

    .project-description {
        margin: 0.5rem auto;
        max-width: 750px;
    }
    
</style>