<script lang="ts">
	import { page } from '$app/stores';

	$: url = $page.url;
	$: status = url.searchParams.get('status'); // 'success' or 'error'
	$: reason = url.searchParams.get('reason'); // optional
</script>

<h1 class="text-2xl font-bold">Google OAuth Result</h1>

{#if status === 'success'}
	<p class="text-green-600">✅ Login successful! You're all set.</p>
{:else if status === 'error'}
	<p class="text-red-600">❌ Login failed. Reason: {reason ?? 'unknown error'}</p>
{:else}
	<p>Waiting for redirect result...</p>
{/if}
