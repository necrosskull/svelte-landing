<script>
	import { onMount } from 'svelte';
	let constrain = 150;
	let ex1Layer;
	let isColorInverted = false;

	function toggleColor() {
		isColorInverted = !isColorInverted;
	}

	const links = [
		{
			name: 'Telegram',
			url: 'https://t.me/necrosskull'
		},
		{
			name: 'GitHub',
			url: 'https://github.com/necrosskull'
		}
	];

	function transformElement(el, x, y) {
		const box = el.getBoundingClientRect();
		const calcX = -(y - box.y - box.height / 2) / constrain;
		const calcY = (x - box.x - box.width / 2) / constrain;
		el.style.transform = `perspective(100px) rotateX(${calcX}deg) rotateY(${calcY}deg)`;
	}

	onMount(() => {
		ex1Layer = document.getElementById('ex1-layer');
		const container = document.getElementById('ex1');
		let isTouching = false;

		const touchStartHandler = (e) => {
			isTouching = true;
			const touch = e.touches[0];
			transformElement(ex1Layer, touch.clientX, touch.clientY);
			transformBackground(touch.clientX, touch.clientY);
		};

		const touchMoveHandler = (e) => {
			if (isTouching) {
				const touch = e.touches[0];
				transformElement(ex1Layer, touch.clientX, touch.clientY);
				transformBackground(touch.clientX, touch.clientY);
			}
		};

		const touchEndHandler = () => {
			isTouching = false;
			ex1Layer.style.transform = 'perspective(1000px) rotateX(0deg) rotateY(0deg)';
		};

		container.addEventListener('touchstart', touchStartHandler);
		container.addEventListener('touchmove', touchMoveHandler);
		container.addEventListener('touchend', touchEndHandler);

		const mouseMoveHandler = (e) => {
			const { clientX, clientY } = e;
			transformElement(ex1Layer, clientX, clientY);
			transformBackground(clientX, clientY);
		};

		const background = document.getElementById('background');

		function transformBackground(x, y) {
			const calcX = -(y - window.innerHeight / 2) / constrain + 10;
			const calcY = -(x - window.innerWidth / 2) / constrain + 10;
			background.style.transform = `perspective(1000px) translateX(${calcY}px) translateY(${calcX}px)`;
		}

		container.addEventListener('mousemove', mouseMoveHandler);
	});
</script>

<svelte:head>
	<title>necrosskull</title>
</svelte:head>

<main
	id="ex1"
	class="bg-black absolute inset-0 z-0 container min-h-[calc(100dvh)] flex items-center justify-center min-w-full overflow-hidden"
>
	<div
		id="background"
		style="background-image: url('/necrosskull.png')"
		class="absolute inset-0 z-10 bg-cover bg-center brightness-50 opacity-60 blur-3xl"
	/>
	<div
		id="ex1-layer"
		class="flex items-center justify-center z-20 text-pink-100 transform-gpu ease-linear transition-transform"
	>
		<div class="flex flex-col py-2 px-6 rounded-xl lg:py-6 lg:px-8 select-none">
			<div class="flex flex-col text-center mb-4 text-5xl lg:text-6xl">
				{#each Array(6) as _}
					<div>
						<span
							class="font-bold {!isColorInverted
								? 'text-purple-600'
								: 'text-purple-100'} transition-colors duration-500">n</span
						><span
							class="font-bold {isColorInverted
								? 'text-purple-600'
								: 'text-purple-100'} transition-colors duration-500">ecrossk</span
						><span
							class="font-bold {!isColorInverted
								? 'text-purple-600'
								: 'text-purple-100'} transition-colors duration-500">ull</span
						>
					</div>
				{/each}
			</div>
			<div class="flex justify-center text-center mb-4 lg:text-2xl">
				<button
					on:click={toggleColor}
					class="bg-purple-900 hover:bg-purple-400 hover:text-purple-950 active:bg-purple-900 active:text-purple-100 rounded-xl p-2 w-full font-bold transition-colors duration-300"
				>
					Click me
				</button>
			</div>
			<div class="flex justify-center text-center space-x-2 mb-4 lg:text-2xl">
				{#each links as link}
					<a
						href={link.url}
						target="_blank"
						class="bg-purple-900 hover:bg-purple-400 hover:text-purple-950 transition-colors duration-300 rounded-xl p-2 w-1/2 font-bold"
					>
						{link.name}
					</a>
				{/each}
			</div>
		</div>
	</div>
</main>

<style>
	.container {
		height: 100dvh;
		width: 100vw;
		display: flex;
		align-items: center;
		justify-content: center;
	}
</style>
