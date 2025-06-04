<script lang="ts">
	import { page } from '$app/state';

</script>

<svelte:head>
    <title>{page.status}</title>
    <meta property="og:title" content="{page.status} " />
    <meta name="twitter:title" content="{page.status} " />
</svelte:head>

<div class="w-full h-full pointer-events-none relative vigenette bg-cover bg-[image:url('https://rerigferl.github.io/ogp.webp')]">
</div>
