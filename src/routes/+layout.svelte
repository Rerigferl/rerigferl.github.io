<script lang="ts">
	import '../app.css';
	export const prerender = true;
	
	let { children } = $props();

</script>

<svelte:head>
	<meta property="og:description" content="Web site" />
	<meta property="og:type" content="website" />
	<meta property="og:url" content="https://rerigferl.github.io/" />
	<meta property="og:image" content="https://rerigferl.github.io/ogp.webp" />
	<meta property="og:image:width" content="1200" />
	<meta property="og:image:height" content="630" />
	<meta property="og:locale" content="ja_JP" />
	<meta name="twitter:card" content="summary_large_image" />
	<meta name="twitter:description" content="Web site" />
	<meta name="twitter:image" content="https://rerigferl.github.io/ogp.webp" />
	<meta name="twitter:site" content="@Gomorroth" />
</svelte:head>

{@render children()}
