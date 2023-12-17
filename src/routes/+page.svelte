<script lang="ts">
	import App from '$lib/components/App.svelte'
	import { RangeSlider } from '@skeletonlabs/skeleton'

	let value = 7
	let max = 10
</script>

<div class="main-container">
	<!-- svelte main -->
	<div class="w-full h-[30vh] flex justify-center items-center">
		<!-- first section -->
		<section class="">
			<h1 class="bg-primary-300">Hey There</h1>
		</section>

		<!-- slider -->
		<RangeSlider
			name="range-slider"
			bind:value={value}
			max={max}
			step={1}
      on:change={e => console.log(e)}
			ticked
		>
			<div class="flex justify-between items-center">
				<div class="font-bold">Label</div>
				<div class="text-xs">{value} / {max}</div>
			</div>
		</RangeSlider>
	</div>

	<!-- threlte main -->
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
