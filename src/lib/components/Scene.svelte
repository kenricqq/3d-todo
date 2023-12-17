<script lang="ts">
	import { T } from '@threlte/core'
	import { HTML, Float, OrbitControls, interactivity } from '@threlte/extras'
	import { spring } from 'svelte/motion'
	import IBox from './IBox.svelte'
	import Todo from './Todo.svelte'

	export let todos = [
		{ id: 1, text: 'Learn Svelte' },
		{ id: 2, text: 'Learn Three.js' },
		{ id: 3, text: 'Build a cool app' },
	]

	const { target } = interactivity()
	target.set(document.getElementById('int-target') ?? undefined)

	const pos = spring({ x: 2, z: 1 })
	const setRandomPos = () => {
		pos.set({
			x: (Math.random() - 0.5) * 5,
			z: (Math.random() - 0.5) * 5,
		})
	}

	const onClick = () => {
		console.log('HI THERE')
	}
	let isHovering = false
	let isPointerDown = false

	let inputText = 'todo'

	const handleSubmit = () => {
		todos = [...todos, { id: Math.random(), text: inputText }]
		inputText = ''
		console.log(todos)
	}
</script>

<Todo todoText={'hi there'} y={5} z={1} />

<T.PerspectiveCamera makeDefault position={[0, 5, 35]} fov={25}>
	<OrbitControls enableDamping autoRotateSpeed={0.5} target.y={1.5} />
</T.PerspectiveCamera>

<T.DirectionalLight intensity={0.8} position.x={5} position.y={10} />
<T.AmbientLight intensity={0.2} />

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

<T.Mesh>
	<HTML position.y={2} position.z={2} transform>
		<button
			on:pointerenter={() => (isHovering = true)}
			on:pointerleave={() => {
				isPointerDown = false
				isHovering = false
			}}
			on:pointerdown={() => (isPointerDown = true)}
			on:pointerup={() => (isPointerDown = false)}
			on:pointercancel={() => {
				isPointerDown = false
				isHovering = false
			}}
			on:click={onClick}
			class="bg-orange-500 rounded-full px-3 text-white hover:opacity-90 active:opacity-70"
		>
			I'm a regular HTML button
		</button>

		<form on:submit={handleSubmit}>
			<input
				type="text"
				bind:value={inputText}
				placeholder="Type something..."
			/>
			<button type="submit">Submit</button>
		</form>

		{#each todos as todo}
			<h2>{todo.text}</h2>
		{/each}
	</HTML>
</T.Mesh>

<IBox on:click={setRandomPos} position.x={$pos.x} position.z={$pos.z} />
