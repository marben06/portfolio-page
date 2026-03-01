<script>
    import { marked } from 'marked';
    import { tick } from 'svelte';
    import { onMount } from 'svelte';

    // server setup
    const apiKey = import.meta.env.VITE_API_KEY;
    const baseUrl = import.meta.env.DEV ? import.meta.env.VITE_SERVER_URL_DEV : import.meta.env.VITE_SERVER_URL;

    let { toggleChat } = $props();
    let messages = $state([]);
    let input = $state('');
    let chatInput;
    let loading = $state(false);
    let loadingTooLong = $state(false);

    onMount(() => { 
        chatInput.focus();

        //if a response takes too long another loading message should be displayed
        const timeout = setTimeout(() => {
            loadingTooLong = true;
        }, 6000); 

        return () => clearTimeout(timeout);
    
    })

    const scrollToLastMessage = () => {
        const elements = document.querySelectorAll('.message');
        const last = elements[elements.length - 1];
        last?.scrollIntoView({ behavior: 'smooth', block: 'end' });
    }

    const sendMessage = async () => {
        if (!input.trim()) return;
        messages = [...messages, { role: 'user', text: input }];
        await tick();
        scrollToLastMessage();
        loading = true;
        input = '';

        const res = await fetch(`${baseUrl}portfolio-chat`, {
            method: 'POST',
            headers: { 
                'Content-Type': 'application/json',
                "x-api-key": apiKey
            },
            body: JSON.stringify({ message: messages.at(-1).text })
        });

        const data = await res.json();
        messages = [...messages, { role: 'assistant', text: data.reply }];
        await tick();
        scrollToLastMessage();
        loading = false;
    }
</script>

<div id="chat-container">
    <div id="chat-window">
    <button id="close-button" onclick={toggleChat}>X</button>
   {#each messages as msg}
        <div class="message {msg.role}">
            {@html marked(msg.text)}
        </div>
    {/each}

    {#if loading}
        <p class="loading-indicator">Laden...</p>
        {#if loadingTooLong}
            <p class="loading-indicator">Mein kostenfreier Render Server schläft noch...</p>
            <p class="loading-indicator">Ich brauche maximal eine Minute um aufzuwachen...</p>
            <p class="loading-indicator">Ich bitte um etwas Geduld...</p>
        {/if}
    {/if}
</div>
    <form onsubmit={sendMessage}>
        <label for="question">Stelle deine Frage zu den Projekten:</label>
        <input 
            bind:this={chatInput} 
            id="question" 
            name="question" 
            bind:value={input} />
        <button type="submit">Senden</button>
    </form>
</div>

<style>
    #chat-container {
        position: fixed;
        z-index: 1000;
        height: 85vh;
        background-color: var(--background);
        border: solid 0.2rem var(--white);
        bottom: 10px;
        right: 4%;
        display: flex;
        flex-direction: column;
        width: 90%;
        max-width: 800px;
        margin: auto;

    }

    #close-button {
        width: 30px;
        height: 30px;
        font-size: 0.8rem;
        border-radius: 100%;
        color: var(--background);
        border: solid 0.1rem var(--background);
        text-align: center;
        padding: 0;
        position: absolute;
        left: 10px;
        top: 10px;
        transition: all 0.1s;
    }

    #close-button:hover {
        font-weight: 700;
         border-width: 0.17rem;
    }

    #chat-window {
        flex: 1;
        background-color: var(--white);
        display: flex;
        flex-direction: column;
        overflow: hidden;
        overflow-y: scroll;
    }

    form {
        margin: 0.8rem;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-evenly;
        gap: 1rem;
        margin: 1.8rem;
    }

    form label {
        font-size: 0.8rem;
    }

    form input {
        display: block;
        width: 95%;
        max-width: 600px;
        background-color: var(--white);
        height: 2rem;
        border-radius: 15px;
        z-index: 50;
        border: none;
        outline: none;
        appearance: none;
        -webkit-appearance: none;
        padding: 0.4rem;
        font-family: 'Mono Space';
        font-weight: 700;
    }

    form input:focus {
        outline: none;
        box-shadow: none;
    }

    button {
        text-align: center;
        margin-top: 0.7rem;
    }

    .messages {
        display: flex;
        flex-direction: column;
        gap: 1rem;
    }

    p {
        padding: 1rem;
    }

    .message {
        padding: 0.3rem 1rem;
        font-size: 0.8rem;
    }

    .user {
        background: var(--accent);
        color: var(--white);
        height: fit-content;
        align-self: flex-end;
        width: 40%;
        margin: 0.5rem;
        border-radius: 20px;
    }

    .assistant {
        color: var(--background);
        align-self: flex-start;
        border-bottom-left-radius: 0.2rem;
        height: fit-content;
        width: 90%;
        padding-left: 3rem;
    }

    .chat-urls {
        color: var(--background)
    }

    .chat-urls:visited {
        color: var(--background)
    }

    .chat-urls:active {
        color: var(--background)
    }

    .loading-indicator {
        color: var(--background);
        align-self: flex-end;
        padding: 0.5rem;
        margin: 0.5rem;
    }

</style>