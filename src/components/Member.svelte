<script>
    let { name, mc_username, ttv_username, yt_username } = $props();
</script>

<div class="memberBox">
    <img
        src={
            "https://mineskin.eu/helm/"
            + (mc_username ? mc_username : "Alex")}
        alt={ttv_username} width="72" height="72" />
    <div class="memberInfo">
        <span>{name ? name : ttv_username}</span>
        <div class="memberSocmed">
            {#if ttv_username}
                <a href={"https://twitch.tv/" + ttv_username}>
                    <img src="/ttv.svg" alt="Twitch" width="24" style="fill: purple;"/>
                    /{ttv_username}
                </a>
            {:else if yt_username}
                <a href={"https://youtube.com/@" + yt_username}>
                    <img src="/yt.svg" alt="YouTube" height="24" style="fill: purple;"/>
                    @{yt_username}
                </a>
            {/if}
        </div>
    </div>
</div>