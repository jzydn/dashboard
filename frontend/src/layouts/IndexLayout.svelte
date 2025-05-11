<script>
	import { fade, fly } from 'svelte/transition';
	import { onMount } from 'svelte';
	let loaded = false;

	onMount(() => {
		setTimeout(() => {
			loaded = true;
		}, 200);
	});
</script>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap');

	:global(body) {
		margin: 0;
		padding: 0;
		background: linear-gradient(135deg, #0e0e10, #1b1b23);
		font-family: 'Inter', sans-serif;
		color: white;
	}

	.hero {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		text-align: center;
		height: 100%;
		width: 100%;
		padding: 4rem 2rem;
		background: radial-gradient(circle at center, #1e1e2f, #0e0e10);
	}

	.card {
		background: rgba(255, 255, 255, 0.05);
		border: 1px solid rgba(255, 255, 255, 0.08);
		border-radius: 20px;
		backdrop-filter: blur(10px);
		box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
		padding: 3rem;
		max-width: 700px;
		transition: transform 0.3s ease, box-shadow 0.3s ease;
	}

	.card:hover {
		transform: scale(1.02);
		box-shadow: 0 15px 40px rgba(255, 75, 43, 0.25);
	}

	h1 {
		font-size: 3rem;
		font-weight: 800;
		background: linear-gradient(to right, #ff416c, #ff4b2b);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		margin-bottom: 1rem;
	}

	p {
		font-size: 1.25rem;
		color: #cfcfcf;
		margin-bottom: 2rem;
	}

	.actions {
		display: flex;
		justify-content: center;
		gap: 1rem;
		flex-wrap: wrap;
	}

	button {
		background: #ff4b2b;
		color: white;
		border: none;
		padding: 0.75rem 1.5rem;
		border-radius: 12px;
		cursor: pointer;
		font-weight: 600;
		transition: background 0.3s ease, transform 0.3s ease;
		box-shadow: 0 0 15px rgba(255, 75, 43, 0.2);
	}

	button:hover {
		background: #ff416c;
		transform: translateY(-2px);
	}

	@media (max-width: 600px) {
		h1 {
			font-size: 2.25rem;
		}

		p {
			font-size: 1rem;
		}
	}
</style>

<div class="hero">
	{#if loaded}
		<div class="card" in:fade={{ duration: 400 }} in:fly={{ y: 30 }}>
			<h1>Welcome to Your Dashboard</h1>
			<p>Manage your whitelabel Discord bots, track usage, update status, and customize them with style.</p>
			<div class="actions">
				<button on:click={() => alert("Navigate to bot setup")}>Launch Bot</button>
				<button on:click={() => alert("Navigate to logs")}>View Logs</button>
				<button on:click={() => alert("Navigate to settings")}>Settings</button>
			</div>
		</div>
	{/if}
</div>
