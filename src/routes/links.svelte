<script module lang="ts">
    import type { Snippet } from 'svelte';

    export { Links };

    type Item = {
        title: string;
        logo: Snippet;
        link: string;
        displayText?: string;
    };

    const items: Item[] = [
        {title: "Twitter", link: "https://x.com/Gomorroth", logo: TwitterLogo, displayText: "@Gomorroth"},
        {title: "Misskey", link: "https://misskey.niri.la/@numeira", logo: MisskeyLogo, displayText: "Misskey (niri.la)"},
        {title: "GitHub", link: "https://github.com/Rerigferl", logo: GitHubLogo, displayText: "GitHub"},
        {title: "Booth", link: "https://ashley-scarlet.booth.pm/", logo: BoothLogo, displayText: "Booth"},
        {title: "Fanbox", link: "https://rerigferl.fanbox.cc/", logo: FanboxLogo, displayText: "Fanbox"},
    ];

</script>

{#snippet TwitterLogo()}
        <img class=" invert" src="/logo/twitter.svg" alt="twitter icon"/>
{/snippet}

{#snippet MisskeyLogo()}
        <img class=" brightness-200" src="/logo/misskey.png" alt="misskey icon"/>
{/snippet}

{#snippet GitHubLogo()}
        <img src="/logo/GitHub.svg" alt="github icon"/>
{/snippet}

{#snippet BoothLogo()}
        <img src="/logo/booth.svg" alt="booth icon"/>
{/snippet}

{#snippet FanboxLogo()}
        <img src="/logo/pixiv.svg" alt="pixiv icon"/>
{/snippet}

{#snippet Links()}
    <div class="relative w-full h-full">
        <div class="absolute flex flex-row w-full h-full p-8">
            <div class=" w-full h-full gap-8 justify-center justify-items-center items-center
                flex flex-col 
                md:grid grid-cols-2 grid-rows-3">
                {#each items as item, i}
                <div class="flex justify-center items-center w-full lg:h-full shrink-0 ">
                    <span class="flex w-[12rem] h-full gap-2 justify-center items-center 
                                 row-start-[{(i % 3) + 1}] col-start-[{(i < 3 ? 1 : 2)}]">
                        <a class="flex justify-center items-center align-middle self-center w-10 h-10 fill-[#f3f3f3] bg-[#3e3e3e] p-1 rounded-md grayscale shrink-0" href={item.link} target="_blank">
                            {@render item.logo()}
                        </a>
                        <a class="h-full w-full flex items-center pointer-events-none" href={item.link} target="_blank">
                            <p class="text-xl font-thin tracking-widest w-fit pointer-events-auto">{item.displayText ?? item.link}</p>
                        </a>
                    </span>
                </div>
                {/each}
            </div>
        </div>
    </div>
{/snippet}