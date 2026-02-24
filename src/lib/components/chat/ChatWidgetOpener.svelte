<script>
    let { toggleChat, chatActive } = $props();
</script>

<button onclick={toggleChat}>
    Fragen?
</button>

<style>
    button {
        position: fixed;
        right: 10px;
        bottom: 10px;
        z-index: 900;
    }
</style>