<script>
    import Member from "./Member.svelte";
    let selectedStream = $state("");
    async function fetchStreams() {
        const fetchStreamsResponse = await fetch("https://bsmplive-prod.mdkj.dev/api/list");
        return await fetchStreamsResponse.json();
        //console.log(streamsResponse.data[0].user_login);
    }
    let promise = $state(fetchStreams());
</script>

{#await promise}
    <p>Loading members...</p>
    <div id="loadingBar">
        <div id="loadingBarFill"></div>
    </div>
{:then streams}
    {#if streams[0]}
        <script>
            console.log("[BSMPLIVE] members loaded");
        </script>
        <div id="memberList">
            {#each streams as stream}
                <Member {...stream} />
            {/each}
        </div>
    {:else}
        <h2>-ERROR</h2>
        <p>List not working</p>
    {/if}
{:catch error}
    <h2>-ERROR</h2>
    <p>{error.message}</p>
{/await}