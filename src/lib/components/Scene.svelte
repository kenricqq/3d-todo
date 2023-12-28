<script lang="ts">
	import { onMount } from 'svelte'
	import { T } from '@threlte/core'
	import { HTML, Float, OrbitControls, interactivity } from '@threlte/extras'
	import { spring } from 'svelte/motion'
	import IBox from './IBox.svelte'
	import DText from './3DText.svelte'
	import Todo from './Todo.svelte'
	import { todos } from '$stores/todoStore.js'
	import { addTodo } from '$stores/todoStore'

	onMount(() => {
		const savedTodos = localStorage.getItem('todos')
		if (savedTodos) {
			todos.set(JSON.parse(savedTodos))
		}
	})

	const { target } = interactivity()
	target.set(document.getElementById('int-target') ?? undefined)

	const pos = spring({ x: 2, z: 1 })
	const setRandomPos = () => {
		pos.set({
			x: (Math.random() - 0.5) * 5,
			z: (Math.random() - 0.5) * 5,
		})
	}

	let isHovering = false
	let isPointerDown = false

	let inputText = ''

	const handleSubmit = () => {
		addTodo(inputText)
		console.log($todos)
		inputText = ''
	}
</script>

<DText todoText={'hi there'} y={5} z={1} />

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
		<form on:submit={handleSubmit}>
			<input
				type="text"
				bind:value={inputText}
				placeholder="make a wish"
				class="btn"
			/>
			<button type="submit" class="btn variant-filled-primary">Submit</button
			>
		</form>

		{#each $todos as todo}
			<Todo todo={todo} />
		{/each}
	</HTML>
</T.Mesh>

<IBox on:click={setRandomPos} position.x={$pos.x} position.z={$pos.z} />
