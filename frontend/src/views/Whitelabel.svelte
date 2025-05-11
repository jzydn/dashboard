<script>
	import { fly, fade, scale } from 'svelte/transition';
	import { onMount } from 'svelte';

	let show = false;

	onMount(() => {
		show = true;
	});

	function goHome() {
		window.location.href = '/';
	}
</script>

<style>
	@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;800&display=swap');

	:global(body) {
		margin: 0;
		padding: 0;
		font-family: 'Inter', sans-serif;
		background: linear-gradient(135deg, #0f0f10, #1a1a1d);
		color: white;
	}

	.container {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		min-height: 100vh;
		text-align: center;
		padding: 2rem;
		background: radial-gradient(circle at top left, #2a2a40 0%, #0f0f10 100%);
		position: relative;
		overflow: hidden;
	}

	h1 {
		font-size: 4rem;
		font-weight: 800;
		background: linear-gradient(to right, #ff416c, #ff4b2b);
		-webkit-background-clip: text;
		-webkit-text-fill-color: transparent;
		margin-bottom: 0.5rem;
		animation: pulse 2s infinite ease-in-out;
	}

	h2 {
		font-size: 1.25rem;
		font-weight: 400;
		color: #cccccc;
		margin-bottom: 2rem;
		max-width: 600px;
	}

	button {
		background: #ff4b2b;
		border: none;
		padding: 0.75rem 1.5rem;
		border-radius: 10px;
		color: white;
		font-weight: 600;
		cursor: pointer;
		transition: background 0.3s ease;
		box-shadow: 0 0 20px rgba(255, 75, 43, 0.4);
	}

	button:hover {
		background: #ff416c;
	}

	.illustration {
		width: 350px;
		max-width: 80%;
		margin: 2rem 0;
		opacity: 0.9;
		filter: drop-shadow(0 0 10px #ff416c);
		transition: transform 0.4s ease;
	}

	.illustration:hover {
		transform: scale(1.05);
	}

	@keyframes pulse {
		0% { text-shadow: 0 0 10px rgba(255, 75, 43, 0.6); }
		50% { text-shadow: 0 0 20px rgba(255, 75, 43, 1); }
		100% { text-shadow: 0 0 10px rgba(255, 75, 43, 0.6); }
	}

	@media (max-width: 600px) {
		h1 {
			font-size: 3rem;
		}
		h2 {
			font-size: 1rem;
		}
	}
</style>

<div class="container" transition:fade={{ duration: 800 }}>
	{#if show}
		<img
			src="https://assets10.lottiefiles.com/packages/lf20_touohxv0.json"
			alt="Whitelabel 404"
			class="illustration"
			use:fade
			on:load
		/>

		<h1 in:fly={{ y: -20, duration: 600 }}>Whitelabel Unavailable</h1>
		<h2 in:fly={{ y: 20, duration: 600, delay: 200 }}>
			The whitelabel bot service isn’t available right now — we're cooking something hot 🔥. Check back soon.
		</h2>

		<button on:click={goHome} in:scale={{ duration: 400, delay: 400 }}>
			Back to Home
		</button>
	{/if}
</div>
