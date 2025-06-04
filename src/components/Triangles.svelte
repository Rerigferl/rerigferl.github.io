<script lang="ts">
    import { onMount } from "svelte";
    import Triangle from "./Triangle.svelte";
    
    let {fill} : {fill?: string} = $props();

	let innerWidth = $state(0);
    let innerHeight = $state(0);
    
    const count = 30;

    let baseSize = $derived(Math.max(innerWidth, innerHeight) / count);

    let colCount = $derived(innerWidth > innerHeight ? count + 1 : count + 1);
    let rowCount = $derived(innerWidth > innerHeight ? innerHeight / (baseSize * 0.8) + 1 : innerHeight / (baseSize * 0.8) + 1);

    let seed = $state(12);

    onMount(() => {
        setTimeout(() => {
            seed = 0.2;
        }, 500);
        const cancel = setInterval(() => {
            seed = 0.2 + Math.random() * 0.7;
        }, 5000);
        return () => clearInterval(cancel);
    });

</script>

<svelte:window bind:innerWidth bind:innerHeight/>

<div class="w-full h-full flex flex-col select-none ">
{#each {length: rowCount}, y }
    {@const offset = y % 2 != 0 ? `--tw-translate-x: -${baseSize / 2}px;` : ""}
    <div class="flex flex-row translate-x-0" style="{offset}">
    {#each {length: colCount}, x }
        {@const rand = Math.random()}
        {@const isVisible = rand > seed + ((innerWidth > innerHeight ? (x >= rowCount - y) : (y >= x)) ? 0 : 0.65 )}
            <div class="pointer-events-auto block shrink-0 overflow-hidden transition-transform duration-500 ease-out {isVisible ? "scale-100" : "scale-0"} hover:scale-0 hover:delay-75" style="fill: {fill};">
                <Triangle width={baseSize}/>
            </div>
    {/each}
</div>
{/each}
</div>