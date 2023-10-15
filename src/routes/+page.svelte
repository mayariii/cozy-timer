<script lang="ts">
	import { Canvas } from '@threlte/core';
	import { Sky } from '@threlte/extras';
	import Room from '../components/Room.svelte';
	import Pond from '../components/Pond.svelte';

	let scene: 'room' | 'pond' = 'room';
	let lightLevel: 'dim' | 'bright';
	let autoRotate = false;

	let minutes = 25;
	let seconds = 0;
	let isRunning = false;
	let timer: number | undefined;

	function startTimer() {
		isRunning = true;
		timer = setInterval(() => {
			if (seconds === 0) {
				if (minutes === 0) {
					clearInterval(timer);
					isRunning = false;
					alert('Pomodoro done!');
				} else {
					minutes -= 1;
					seconds = 59;
				}
			} else {
				seconds -= 1;
			}
		}, 1000);
	}

	function stopTimer() {
		clearInterval(timer);
		isRunning = false;
	}

	function setTimer(mins: number) {
		clearInterval(timer);
		isRunning = false;
		minutes = mins;
		seconds = 0;
	}

	function resetTimer() {
		clearInterval(timer);
		isRunning = false;
		minutes = 25;
		seconds = 0;
	}

	function toggleLightLevel() {
		lightLevel = lightLevel === 'dim' ? 'bright' : 'dim';
	}

	const TIME_BUTTON_CLASSES =
		'bg-violet-50/10 text-gray-100 px-2 py-1 rounded-md text-sm text-semibold';
</script>

<svelte:head>
	<title>cozy timer ☁️</title>
	<meta name="description" content="A simple pomodoro timer with cozy vibes" />
</svelte:head>

<div class="h-full">
	<div class="h-full w-full">
		<Canvas>
			{#if scene === 'room'}
				<Sky
					setEnvironment
					elevation={-4}
					turbidity={20}
					rayleigh={0.57}
					azimuth={180}
					mieCoefficient={0.038}
					mieDirectionalG={0} />
				<Room {lightLevel} />
			{:else}
				<Sky
					setEnvironment
					elevation={-5}
					turbidity={20}
					rayleigh={1}
					azimuth={180}
					mieCoefficient={0.038}
					mieDirectionalG={0} />
				<Pond {autoRotate} />
			{/if}
		</Canvas>
	</div>
	<div class="fixed bottom-0 md:bottom-8 w-screen z-10">
		<div class="flex flex-col gap-2 items-center mb-6 md:mb-10">
			<h1 class="text-xl md:text-2xl text-white font-semibold">cozy timer ☁️</h1>
			<div class="flex gap-1 text-violet-50 text-sm items-center">
				<!-- scene options -->
				<p class="font-semibold">scenes:</p>
				<button on:click={() => (scene = 'room')} class="bg-violet-50/10 px-1 py-0.5 rounded-md"
					>home 🏡</button>
				<button on:click={() => (scene = 'pond')} class="bg-violet-50/10 px-1 py-0.5 rounded-md"
					>pond 🪷</button>
			</div>

			<div class="flex gap-1 items-center">
				<!-- timer options -->
				<p class="text-sm text-violet-50 font-semibold">timer:</p>
				<button
					on:click={() => setTimer(5)}
					class={TIME_BUTTON_CLASSES}
					aria-label="set 5 minute timer">5</button>
				<button
					on:click={() => setTimer(10)}
					class={TIME_BUTTON_CLASSES}
					aria-label="set 10 minute timer">10</button>
				<button
					on:click={() => setTimer(25)}
					class={TIME_BUTTON_CLASSES}
					aria-label="set 25 minute timer">25</button>
				<button
					on:click={() => setTimer(50)}
					class={TIME_BUTTON_CLASSES}
					aria-label="set 50 minute timer">50</button>
			</div>
			<!-- timer display -->
			<p class="bg-violet-50/10 text-violet-50 text-2xl md:text-5xl rounded-md p-2 md:p-4 font-semibold">
				{minutes < 10 ? `0${minutes}` : minutes}:{seconds < 10 ? `0${seconds}` : seconds}
			</p>

			<div class="flex gap-1">
				<!-- timer action buttons -->
				<button on:click={resetTimer} class={TIME_BUTTON_CLASSES}>reset</button>
				<button
					on:click={isRunning ? stopTimer : startTimer}
					class=" bg-violet-50/10 px-2 py-1 rounded-md text-sm font-semibold"
					class:text-green-300={!isRunning}
					class:text-red-300={isRunning}>{isRunning ? 'pause' : 'start'}</button>
			</div>

			<p class="text-gray-100/80 text-xs mt-1 max-w-[30ch] text-center">
				explore scene: drag to rotate, cmd + drag to pan, pinch to zoom in/out
			</p>

			<!-- scene specific options -->
			{#if scene === 'room'}
				<button
					on:click={toggleLightLevel}
					class="bg-violet-50/10 text-violet-50 text-xs px-1 py-0.5 rounded-md"
					>toggle the big lights</button>
			{/if}
			{#if scene === 'pond'}
				<button
					on:click={() => (autoRotate = !autoRotate)}
					class="bg-violet-50/10 text-violet-50 text-xs px-1 py-0.5 rounded-md"
					>{`turn ${autoRotate ? 'off' : 'on'} auto rotate`}
				</button>
			{/if}
		</div>
	</div>
</div>
