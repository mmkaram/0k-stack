# The 0k Stack
- Svelte (ts)
- Drizzle
- Postgres
- Better-auth
- Cloudflare workers (wrangler.json)

## Extras
- Custom loading state:
```svelte
<script lang="ts">
	import { navigating } from '$app/state';
	import '../app.css';

	let { children } = $props();

	// Allow for a 150 ms delay between navigation start and spinner activation.
	// This is to avoid jarring split-second spinners.
	let timer: NodeJS.Timeout;
	let showSpinner = $state(false);

	$effect(() => {
		if (navigating.from !== null) {
			clearTimeout(timer);
			timer = setTimeout(() => {
				showSpinner = true;
			}, 150);
		} else {
			clearTimeout(timer);
			showSpinner = false;
		}
	});
</script>

<div>
    <header>
    </header>
	<main class="p-10">
		{#if showSpinner}
			<Spinner />
		{:else}
			{@render children()}
		{/if}
	</main>
</div>
