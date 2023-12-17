<script lang="ts">
	import { onMount } from 'svelte'
	import { FontLoader } from 'three/examples/jsm/loaders/FontLoader.js'
	import { TextGeometry } from 'three/addons/geometries/TextGeometry.js'
	import { T } from '@threlte/core'
	import { useTexture } from '@threlte/extras'

	export let todoText = 'Hello World!'
	export let y = 0
	export let z = 0

	let size = 1
	let height = 0.2

	let textGeometry: TextGeometry
	let loaded = false

	const matcapTexture = useTexture('textures/matcaps/2.png')

	onMount(() => {
		const loader = new FontLoader()
		loader.load('fonts/helvetiker_regular.typeface.json', function (font) {
			textGeometry = new TextGeometry(todoText, {
				font: font,
				size: size,
				height: height,
				curveSegments: 3,
				bevelEnabled: true,
				bevelThickness: 0.001,
				bevelSize: 0.05,
				bevelOffset: 0,
				bevelSegments: 7,
			})

			textGeometry.center()

			loaded = true
		})
	})
</script>

{#if loaded}
	{#await matcapTexture then matcap}
		<T.Mesh
			geometry={textGeometry}
			position.x={0}
			position.y={y}
			position.z={z}
		>
			<!-- <T.MeshStandardMaterial color={color} /> -->
			<T.MeshMatcapMaterial matcap={matcap} />
		</T.Mesh>
	{/await}
{/if}
