<script lang="ts">
	import Header from '$lib/components/Header.svelte';
	import RightBar from '$lib/components/RightBar.svelte';
	import '../app.css';
	import { invalidate } from '$app/navigation';
	import { onMount } from 'svelte';

	export let data;

	let { supabase, session } = data;
	$: ({ supabase, session } = data);

	onMount(() => {
		const { data } = supabase.auth.onAuthStateChange((event, _session) => {
			if (_session?.expires_at !== session?.expires_at) {
				invalidate('supabase:auth');
			}
		});

		return () => data.subscription.unsubscribe();
	});
</script>

<svelte:head>
	<title>Thotsy</title>
</svelte:head>

<div class="mx-auto grid grid-cols-12 lg:container">
	<Header />
	<slot />
	<RightBar />
</div>
