<script lang="ts">
	import App from '$lib/components/App.svelte'
	import { Play, Pause, Github } from 'lucide-svelte'

	let paused = true

	const toggleAudio = () => {
		const audio = document.getElementById('myAudio')
		if (paused) {
			audio.play()
		} else {
			audio.pause()
		}
		paused = !paused
	}
</script>

<div class="main-container">
	<!-- svelte main -->
	<div class="w-full h-[30vh] flex flex-col justify-center items-center">
		<!-- first section -->
		<section class="">
			<h1 class="p-5 mt-8 text-5xl font-semibold">Welcome to 3D Todo!</h1>
			<audio id="myAudio" autoplay loop>
				<source src="abstract.mp3" type="audio/mpeg" />
			</audio>
			<div class="wrapper flex gap-3">
				<div
					class="flex justify-center items-center bg-opacity-50 w-1/2 rounded-lg p-3 bg-secondary-300"
				>
					<h1 class="text-2xl p-2">Music</h1>
					<button
						on:click={toggleAudio}
						class="btn-icon variant-filled-primary"
					>
						{#if paused}
							<Play />
						{:else}
							<Pause />
						{/if}
					</button>
				</div>
				<div
					class="flex justify-center items-center bg-opacity-50 w-1/2 rounded-lg p-3 bg-secondary-300"
				>
					<h1 class="text-2xl p-2">Code</h1>
					<a href="https://github.com/kenricqq/3d-todo" target="_blank" class="btn-icon variant-filled-primary">
						<Github />
					</a>
				</div>
			</div>
		</section>
	</div>

	<!-- 3d main -->
	<div class="h-[70vh] w-[100ww]">
		<App />
	</div>
</div>

<style>
	:global(body) {
		margin: 0;
	}

	.main-container {
		width: 100vw;
		height: 100vh;
		background: rgb(130, 190, 256);
		background: linear-gradient(
			180deg,
			rgba(160, 190, 256, 0.8) 0%,
			rgba(80, 12, 210, 0.8) 100%
		);
	}
</style>
