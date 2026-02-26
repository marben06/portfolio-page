<script>
    let { toggleChat, chatActive } = $props();
</script>

<button onclick={toggleChat}>
    Fragen?
</button>

<style>
    button {
        position: fixed;
        right: 20px;
        bottom: 10px;
        z-index: 900;
    }

    @media screen and (min-width: 600px) {
        button {
            right: 55px;
        }
    }
</style>