<script lang="ts">
    import { onMount } from "svelte";
	import { page } from '$app/state';
    import { Links } from "./links.svelte";
    import Triangles from "../components/Triangles.svelte";
    import { goto } from "$app/navigation";
    
    let container: HTMLDivElement | null = $state(null);
    let curtain: HTMLDivElement | null = $state(null);
    let lastOffset = 0;
    let delta = 0;
    let innerWidth = $state(1920);
    let innerHeight = $state(1080);
    let instantScroll = $state(true);
    let index = $state(0);
    let namesArray = $state(([["Gomorroth", 0.1], ["Numeira", 0.5],[ "Rerigferl", 0.5], ["Rinna Koharu", 0.5], ["Xhergractferl", 0.5], ["Ashley Scarlet", 0.6], ["Enxhelious", 0.6], ["Kujira Kujime", 0.9], ["Aetiocetus", 0.9] ] as [string, number][]).sort((x, y) => (Math.random() + x[1]) - (Math.random() + y[1])).map(x => x[0]));

    let title = $state(".");

    $effect(() => {
        goto(`#${container?.children[index].id}`, { replaceState: true });
    });

    let ticking = false;

    onMount(() => {

        const hash = page.url.hash;
        if (hash != null)
        {
            const children = container?.children ?? [];
            for (let i = 0; i < children.length; i++) {
                if (hash == `#${children[i].id}`)
                {
                    index = i;
                    break;   
                }
            }
        }
        function onWheel(e: Event)
        {
            e.preventDefault();

            if (ticking)
                return;
            ticking = true;

            if (e instanceof WheelEvent)
            {
                delta = e.deltaY;
            }
            else if (e instanceof TouchEvent) {
                delta = lastOffset - e.touches[0].clientY;
            }
            requestAnimationFrame(onWheelCore);
        }

        function onWheelCore()
        {
            instantScroll = false;
            if (container == null)
                return;
            let i = index;
            if (delta < -10)
            {
                i--;
            }
            else if (delta > 10) {
                i++;
            }
            index = Math.max(0, Math.min(i, container.children.length - 1));
            setTimeout(() => ticking = false, 500);
        }

        container?.addEventListener("wheel", onWheel);
        container?.addEventListener("touchstart", e => lastOffset = e.touches[0].clientY);
        container?.addEventListener("touchmove", onWheel);
        
        if (curtain != null)
            curtain.style = `background-color: transparent; pointer-events: none`;

        title = namesArray[0];
    });

    const ScreenType = {
        Landscape: 0,
        Portrait: 1,
    } as const;

    let screenType = $derived.by(() => {
        const aspectRatio = innerHeight / innerWidth;
        return aspectRatio > (4/3) ? ScreenType.Portrait : ScreenType.Landscape;
    });

</script>

<svelte:window bind:innerWidth bind:innerHeight/>
<svelte:head>
    <title>{title}</title>
    <meta property="og:title" content={title} />
    <meta name="twitter:title" content={title} />
</svelte:head>

<div class="w-full h-full pointer-events-none relative">
    <div class="absolute w-full h-full  overflow-hidden pointer-events-auto {!instantScroll && "scroll-smooth"}" bind:this={container}>
        <div id="main" class="relative overflow-hidden w-full h-full">
            <div class="absolute overflow-hidden w-full h-full">
                <Triangles fill="#e9e4e4"/>
            </div>
            <div class="absolute w-full h-full bg-cover bg-no-repeat pointer-events-none" style="
            {screenType == ScreenType.Portrait ? 
                "background-image: url('./bg-limonene-v.webp'); background-position: center 70%;" : 
                "background-image: url('./bg-limonene.webp'); background-position: 70% center;"}
            ">
            </div>
            <div class="relative w-full h-full">
                <div class="absolute flex flex-col w-full h-full pointer-events-none p-4">
                    <h1 class="text-[clamp(3rem,1.7667rem+5.3333vw,4rem)] font-thin tracking-widest">{namesArray[0]}</h1>
                    <p class="bg-black text-nowrap text-white p-1 px-4 w-max text-2xl font-light tracking-widest">{namesArray[1]} / {namesArray[2]}</p>
                </div>
            </div>
        </div>

        <div id="links" class="relative overflow-hidden w-full h-full bg-white">
            {@render Links()}
        </div>
    </div>

    <div class="absolute w-full h-full flex flex-col items-end justify-center">
        {#each {length: container?.children.length}, i }
            <button class="block w-3 h-3 rounded-full m-2 pointer-events-auto transition-colors duration-500 cursor-pointer {i == index ? " bg-cyan-600" : " bg-white/80"} border-[0.5px] {i == index ? "border-transparent" : " border-cyan-600"}" aria-label="navigate to page {i + 1}" onclick={() => index = i}></button>
        {/each}
    </div>

    <div class="pointer-events-auto absolute w-full h-full bg-white transition-colors delay-200 duration-500" bind:this={curtain}></div>

</div>
