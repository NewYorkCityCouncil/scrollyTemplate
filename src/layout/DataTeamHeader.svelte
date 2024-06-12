<script>
    import { themes } from "../config.js";
    import { getContext } from "svelte";

    export let theme = getContext("theme");
    export let filled = false;
    export let center = true;
</script>

<nav
    style="border-bottom-color: {themes[theme]['muted']}; {filled
        ? 'background-color: ' + themes[theme]['background'] + ';'
        : ''}"
>
    <div class="col-wide middle" class:center>
        <!-- Code below places data team logo in the website header
        and links to the data team homepage-->
        <a href="https://council.nyc.gov/data/" target="_blank">
            <picture>
                {#if theme == "dark"}
                    <img
                        src="./img/DataTeamLogo.png"
                        alt="New York City Council Data Team"
                    />
                {:else if filled == true}
                    <img
                        src="./img/DataTeamLogo.png"
                        alt="New York City Council Data Team"
                    />
                {:else}
                    <img
                        src="./img/DataTeamLogo.png"
                        alt="New York City Council Data Team"
                    />
                {/if}
            </picture>
        </a>
    </div>
</nav>

<style>
    nav {
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        position: relative;
        height: 75px;
        border-bottom: 1px solid #777;
        z-index: 1;
    }
    picture {
        position: relative;
        top: 0px;
        padding: 0 5px;
    }
    img {
        width: 330px;
    }
    a img:hover {
        cursor: pointer;
    }
</style>
