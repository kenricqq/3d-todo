<script lang="ts">
	import { T } from '@threlte/core'
	import {
		ContactShadows,
		Float,
		Grid,
		OrbitControls,
		interactivity,
	} from '@threlte/extras'
	import { spring } from 'svelte/motion'
	import Box from './Box.svelte'
	import Todo from './Todo.svelte'

	export let todos = [
		{ id: 1, text: 'Learn Svelte', y: 0 },
		{ id: 2, text: 'Learn Three.js', y: 1.5 },
		{ id: 3, text: 'Build a cool app', y: 3 },
	]

	const { target } = interactivity()
	target.set(document.getElementById('int-target') ?? undefined)

	const pos = spring({ x: 0, z: 0 })
	const setRandomPos = () => {
		pos.set({
			x: (Math.random() - 0.5) * 5,
			z: (Math.random() - 0.5) * 5,
		})
	}
</script>

{#each todos as todo}
	<Todo todoText={todo.text} yPos={todo.y}/>
{/each}

<T.PerspectiveCamera makeDefault position={[0, 5, 15]} fov={25}>
	<OrbitControls enableDamping autoRotateSpeed={0.5} target.y={1.5} />
</T.PerspectiveCamera>

<T.DirectionalLight intensity={0.8} position.x={5} position.y={10} />
<T.AmbientLight intensity={0.2} />

<Grid
	position.y={-0.001}
	cellColor="#ffffff"
	sectionColor="#ffffff"
	sectionThickness={0}
	fadeDistance={25}
	cellSize={2}
/>

<ContactShadows scale={10} blur={2} far={2.5} opacity={0.5} />

<!-- TorusKnot -->
<Float floatIntensity={1} floatingRange={[0, 1]}>
	<T.Mesh
		position={[1.2, 1.5, 0.75]}
		rotation.x={5}
		rotation.y={71}
		on:click={setRandomPos}
	>
		<T.TorusKnotGeometry args={[0.5, 0.15, 100, 12, 2, 3]} />
		<T.MeshStandardMaterial color="#F85122" />
	</T.Mesh>
</Float>

<Box on:click={setRandomPos} position.x={$pos.x} position.z={$pos.z} />
