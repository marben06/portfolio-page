<script>
    let { stackitems } = $props();
</script>

<ul 
    class="stack-container" 
    aria-label="tech stack of project:"
>
    {#each stackitems as stackitem}
        <li class="stack-item">
            <span>{stackitem}</span>
        </li>
    {/each}
</ul>

<style>
    .stack-container {
        padding: 1.3rem;
        display: flex;
        flex-wrap: wrap;
        gap: 1rem;
        margin: 3rem auto;
        justify-content: center;
        list-style: none;
    }

    .stack-item {
        font-size:  0.7rem;
        background-color: var(--white);
        color: var(--background);
        padding: 0.4rem 0.6rem;
        border-radius: 0.4rem;
        box-shadow: rgb(204, 219, 232) 3px 3px 6px 0px inset, rgba(255, 255, 255, 0.5) -3px -3px 6px 1px inset;
        /*box-shadow: rgba(0, 0, 0, 0.06) 0px 2px 4px 0px inset;*/
        /*rgba(50, 50, 93, 0.25) 0px 30px 60px -12px inset, rgba(0, 0, 0, 0.3) 0px 18px 36px -18px inset;*/
        cursor: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 32 32"><circle cx="16" cy="16" r="15" fill="none" stroke="%235F9EA0" stroke-width="2"/></svg>'), auto;    }

    @media screen and (min-width: 600px) {
       .stack-container {
            max-width: 600px;
    }
}
</style>