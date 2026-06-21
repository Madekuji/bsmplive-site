<script>
    let selectedStream = $state("");
    async function fetchStreams() {
        const fetchStreamsResponse = await fetch("https://bsmplive-prod.mdkj.dev/api/streams");
        const streamsResponse = await fetchStreamsResponse.json();
        //console.log(streamsResponse.data[0].user_login);
        if(streamsResponse.data[0]) {
            selectedStream = streamsResponse.data[0].user_login;
        }
        if(streamsResponse.data.length === 2) {
            multiviewLink = "https://holodex.net/multiview/AAMYtwitch" + streamsResponse.data[0].user_login + "%2CMAMYtwitch" + streamsResponse.data[1].user_login;
        }
        else {
            multiviewLink = "";
        }
        return streamsResponse.data;
    }
    let promise = $state(fetchStreams());
    let multiviewLink = $state("");
</script>

{#await promise}
    <p>Loading streams...</p>
    <div id="loadingBar">
        <div id="loadingBarFill"></div>
    </div>
{:then streams}
    {#if streams[0]}
        <script>
            console.log("[BSMPLIVE] streams loaded");
        </script>
        <div id="tabsList">
            {#each streams as stream}
                <button
                        class="streamButton"
                        aria-label={stream.user_login}
                        aria-current={selectedStream === stream.user_login}
                        onclick={() => selectedStream = stream.user_login}>{stream.user_login}
                </button>
            {/each}
        </div>
        <!--<p style="margin-top: 0;">Now playing: {selectedStream}</p>-->
        <div id="streamEmbed">
            <iframe src="https://player.twitch.tv/?channel={selectedStream}&parent=bayanihansmp.mdkj.dev&parent=bsmplive-proto.mdkj.dev&parent=localhost" title="twitch embed" allow="fullscreen"></iframe>
        </div>
        {#if multiviewLink}
            <a href={multiviewLink} target="_blank" style="margin-top: 1rem;">
                <button>
                    Multiview
                </button>
            </a>
        {/if}
    {:else}
        <h2>No one is live yet...</h2>
    {/if}
{:catch error}
    <h2>-ERROR</h2>
    <p>{error.message}</p>
{/await}

<div class="buttonsList" style="margin-top: 1rem; justify-content: center;">
    <button onclick={() => promise = fetchStreams()}>
        Refresh streams
    </button>
    <a href="https://twitter.com/hashtag/PHTBayanihanSMPS1" target="_blank">
        <button>#PHTBayanihanSMPS1</button>
    </a>
</div>

