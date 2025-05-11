<script>
	import { onMount } from 'svelte';

	let loaded = false;

	onMount(() => {
		setTimeout(() => {
			loaded = true;
		}, 200);
	});

	// Custom fade + fly transition
	function fadeFly(node, { delay = 0, duration = 400, y = 30 }) {
		return {
			delay,
			duration,
			css: (t, u) => `
				opacity: ${t};
				transform: translateY(${u * y}px);
			`
		};
	}
</script>

<svelte:head>
	<title>Whitelabel Dashboard</title>
</svelte:head>

<div class="hero">
	{#if loaded}
		<div class="card" transition:fadeFly={{ duration: 400, y: 30 }}>
			<h1>🚀 Welcome to Your Whitelabel Dashboard</h1>
			<p>
				Manage your whitelabel Discord bots, track usage, update statuses, and personalize
				them with futuristic flair.
			</p>
			<div class="actions">
				<button on:click={() => alert("Coming soon!")}>Launch Bot</button>
				<button on:click={() => alert("Coming soon!")}>View Logs</button>
				<button on:click={() => alert("Coming soon!")}>Settings</button>
			</div>
		</div>
	{/if}
</div>

<style>
	.hero {
		display: flex;
		align-items: center;
		justify-content: center;
		height: 100vh;
		background: linear-gradient(to bottom right, #1a1a2e, #16213e);
		color: #f5f5f5;
		padding: 2rem;
	}

	.card {
		background: rgba(255, 255, 255, 0.05);
		backdrop-filter: blur(10px);
		padding: 3rem;
		border-radius: 1.5rem;
		box-shadow: 0 20px 50px rgba(0, 0, 0, 0.3);
		text-align: center;
		max-width: 700px;
		width: 100%;
		transition: all 0.3s ease-in-out;
		border: 1px solid rgba(255, 255, 255, 0.1);
	}

	h1 {
		font-size: 2.5rem;
		margin-bottom: 1rem;
		color: #ffffff;
	}

	p {
		font-size: 1.1rem;
		margin-bottom: 2rem;
		color: #cccccc;
	}

	.actions {
		display: flex;
		gap: 1rem;
		justify-content: center;
		flex-wrap: wrap;
	}

	button {
		background: #00b4d8;
		color: white;
		border: none;
		padding: 0.75rem 1.5rem;
		border-radius: 0.75rem;
		font-size: 1rem;
		cursor: pointer;
		transition: background 0.2s ease;
	}

	button:hover {
		background: #0096c7;
	}
</style>
