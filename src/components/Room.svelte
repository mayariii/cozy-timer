<script lang="ts">
	import { T } from '@threlte/core';
	import { OrbitControls } from '@threlte/extras';
	import Scene from './models/room-scene.svelte';

	export let lightLevel: 'dim' | 'bright' = 'bright';
</script>

<T.PerspectiveCamera makeDefault position={[650, 100, 400]} fov={25}>
	<OrbitControls enableDamping />
</T.PerspectiveCamera>

{#if lightLevel === 'bright'}
	<T.AmbientLight />
	<T.DirectionalLight position={[100, 100, 40]} intensity={0.2} />
	<T.DirectionalLight position={[100, 400, 2]} />
{:else if lightLevel === 'dim'}
	<T.DirectionalLight position={[100, 100, 40]} intensity={0.4} />
{/if}

<Scene />
