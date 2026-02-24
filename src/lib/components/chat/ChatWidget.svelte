<script>
    import ChatWidgetOpener from "./ChatWidgetOpener.svelte";
    import ChatWidgetChat from "./ChatWidgetChat.svelte";

    let chatActive = $state(false);

    const toggleChat = () => {
        chatActive = chatActive ? false : true;
    }
</script>

<ChatWidgetOpener {toggleChat} />
{#if chatActive}
    <ChatWidgetChat {toggleChat}/>
{/if}