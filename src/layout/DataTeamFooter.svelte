<script>
    import { themes } from "../config.js";
    import { getContext } from "svelte";

    export let theme = getContext("theme");
</script>

<footer
    style="color: {themes[theme]['text']}; background-color: {themes[theme][
        'pale'
    ]};"
>
    <div class="col-wide" data-analytics="footer">
        <a href="https://council.nyc.gov/data/" target="_blank">
            {#if theme == "dark"}
                <img
                    class="logo-img"
                    src="./img/DataTeamLogo.png"
                    alt="New York City Council Data Team"
                />
            {:else}
                <img
                    class="logo-img"
                    src="./img/DataTeamLogo.png"
                    alt="New York City Council Data Team"
                />
            {/if}
        </a>
        <ul>
            <li>
                <a
                    href="mailto:datainfo@council.nyc.gov"
                    class="link"
                    style="color: {themes[theme]['text']}; font-family:Georgia"
                    >Contact us</a
                >
            </li>
        </ul>
        <hr style="border-top-color: {themes[theme]['muted']}" />
        <div class="license">
            <!-- <p style="font-family:Georgia">
                Any opinion, findings, and conclusions or recommendations
                expressed in this material are those of the authors and do not
                necessarily reflect the views of the NYC Council. All errors are
                our own.
            </p> -->

            <p style="font-family:Georgia">
                Webpage adapted from the template available <a
                    href="https://github.com/onsvisual/svelte-scrolly"
                    target="_blank"
                    style="color: {themes[theme]['text']}">here</a
                >.
            </p>
        </div>
    </div>
</footer>

<style>
    footer {
        padding-bottom: 72px;
        padding-top: 36px;
        font-size: 18px;
    }
    footer a.link {
        -webkit-box-sizing: border-box;
        box-sizing: border-box;
        text-decoration: underline;
        display: inline-block;
    }
    footer a.link:hover {
        text-decoration: none;
    }
    footer a.link:active {
        outline: 3px solid transparent;
        background-color: #fd0;
        -webkit-box-shadow:
            0 -2px #fd0,
            0 4px #222;
        box-shadow:
            0 -2px #fd0,
            0 4px #222;
        color: #222;
        text-decoration: none;
    }
    footer hr {
        margin-bottom: 27px;
        margin-top: 36px;
        border: none;
        border-top: 1px solid #777;
    }
    ul {
        padding: 0;
        margin: 0;
    }
    li {
        margin-right: 18px;
        display: inline-block;
    }
    .logo-img {
        margin-bottom: 27px;
        width: 300px;
    }
    .ogl-img {
        margin: 0 0.5rem 0.2rem 0;
        width: 59px;
        height: 24px;
        vertical-align: middle;
    }
    .license {
        vertical-align: top;
    }
    a img:hover {
        cursor: pointer;
    }
</style>
