<script context="module">
	/** @type {import('@sveltejs/kit').ErrorLoad} */
	export function load({ error, status }) {
		return {
			props: { error, status }
		};
	}
</script>

<script>
	export let status;
	export let error;

	// we don't want to use <svelte:window bind:online> here,
	// because we only care about the online state when
	// the page first loads
	const online = typeof navigator !== 'undefined' ? navigator.onLine : true;
</script>

<svelte:head>
	<title>{status}</title>
</svelte:head>

<div class="container">
	{#if online}
		<h1>Yikes!</h1>

		{#if error.message}
			<p class="error">{status}: {error.message}</p>
		{:else}
			<p class="error">Encountered a {status} error</p>
		{/if}

		{#if import.meta.env.DEV && error.stack}
			<pre>{error.stack}</pre>
		{:else}
			{#if status >= 500}
				<p>Please try reloading the page.</p>
			{/if}

			<p>
				If the error persists, please drop by the <a rel="external" href="/chat">Discord chatroom</a
				>
				and let us know, or raise an issue on
				<a href="https://github.com/sveltejs/svelte">GitHub</a>. Thanks!
			</p>
		{/if}
	{:else}
		<h1>It looks like you're offline</h1>

		<p>Reload the page once you've found the internet.</p>
	{/if}
</div>

<style>
	.container {
		padding-inline: var(--side-nav);
		padding-block: var(--top-offset) 6rem;
	}

	h1,
	p {
		margin-inline: auto;
		margin-block: 0;
	}

	h1 {
		font-size: 2.8em;
		font-weight: 300;
		margin: 0;
		margin-block-end: 0.5em;
	}

	p {
		margin-inline: auto;
		margin-block: 1em;
	}

	.error {
		background-color: var(--second);
		color: white;
		padding-inline: 16px;
		padding-block: 12px;
		font: 600 16px/1.7 var(--font);
		border-radius: 2px;
	}

	/* @media (min-width: 480px) {
		h1 { font-size: 4em }
	} */
</style>
