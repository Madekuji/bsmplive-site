<script>
    let selectedStream = $state("");
    async function fetchStreams() {
        const fetchStreamsResponse = await fetch("https://bsmplive-prod.mdkj.dev/api/streams");
        const streamsResponse = await fetchStreamsResponse.json();
        //console.log(streamsResponse.data[0].user_login);
        if(streamsResponse.data[0]) {
            selectedStream = streamsResponse.data[0].user_login;
        }
        return streamsResponse.data;
    }
    let promise = $state(fetchStreams());
</script>

<h1>Bayanihan SMP Live</h1>

{#await promise}
    <p>Loading stream...</p>
{:then streams}
    {#if streams[0]}
        <p>streams found</p>
        <p>Selected stream: {selectedStream}</p>
        <div id="tabsList">
            {#each streams as stream}
                <button id="streamButton" onclick={() => selectedStream = stream.user_login}>{stream.user_login}</button>
            {/each}
        </div>
        <div id="streamEmbed">
            <iframe src="https://player.twitch.tv/?channel={selectedStream}&parent=bsmplive-proto.mdkj.dev" title="twitch embed" frameborder="0" allowfullscreen="true" scrolling="no" height="378" width="620"></iframe>
        </div>
    {:else}
        <h2>No one is live yet...</h2>
    {/if}
{:catch error}
    <h2>-ERROR</h2>
    <p>{error.message}</p>
{/await}

<button onclick={() => promise = fetchStreams()}>
    Refresh streams
</button>

<footer>
    <p>bsmplive-site prototype 2026-06-16</p>
    <p>built by Madekuji-san</p>
</footer>