<script lang="ts">
	import { onMount } from 'svelte'
	import { FontLoader } from 'three/examples/jsm/loaders/FontLoader.js'
	import { TextGeometry } from 'three/addons/geometries/TextGeometry.js'
	import { T } from '@threlte/core'
	import { useTexture } from '@threlte/extras'
	import { MeshMatcapMaterial } from 'three'

	export let todoText = 'Hello World!'
	export let yPos = 0

	export let size = 1
	export let height = 0.2

	let boundingBox

	let textGeometry: TextGeometry

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

			// get boundingBox, useful for centering
			textGeometry.computeBoundingBox()
			boundingBox = textGeometry.boundingBox
			console.log(boundingBox)

			textGeometry.center()
		})
	})
</script>

{#await matcapTexture then matcap}
	<T.Mesh
		geometry={textGeometry}
		position.x={0}
		position.y={yPos}
		position.z={-2}
	>
		<!-- <T.MeshStandardMaterial color={color} /> -->
		<T.MeshMatcapMaterial matcap={matcap} />
	</T.Mesh>
{/await}
