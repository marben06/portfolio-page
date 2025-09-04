<script>
    let { urls, github } = $props();

</script>

<div class="url-section">
{#each urls as url}
        <div class="url-container">
            <h3>{url[0]}:</h3>
            <div class="project-urls">
                <a class="nav-link" target="_blank" href={url[1][0]}>{url[1][1]}</a>
                {#if github}
                    <a class="nav-link" target="_blank" href={url[2][0]}>{url[2][1]}</a>
                {/if}
            </div>
        </div>
{/each}
    </div>

<style>
    .url-section {
        margin: 1rem auto;
        max-width: 800px;
    }

    .url-container {
        display: flex;
        flex-direction: column;
        align-items: start;
    }

    .project-urls {
        display: flex;
        justify-content: center;
        margin-bottom: 3rem;
        gap: 1rem;
    }

</style>