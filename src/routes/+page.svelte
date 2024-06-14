<script lang="ts">
	import { onMount } from 'svelte';

	let constrain = 150;
	let ex1Layer: HTMLElement | null = null;

	const links = [
		{
			name: 'Telegram',
			url: 'https://t.me/nanazip'
		},
		{
			name: 'GitHub',
			url: 'https://github.com/necrosskull'
		}
	];

	function transformElement(el: HTMLElement, x: number, y: number) {
		const box = el.getBoundingClientRect();
		const calcX = -(y - box.y - box.height / 2) / constrain;
		const calcY = (x - box.x - box.width / 2) / constrain;
		el.style.transform = `perspective(100px) rotateX(${calcX}deg) rotateY(${calcY}deg)`;
	}

	function transformBackground(x: number, y: number) {
		const background = document.getElementById('background') as HTMLElement;
		const calcX = -(y - window.innerHeight / 2) / constrain + 10;
		const calcY = -(x - window.innerWidth / 2) / constrain + 10;
		background.style.transform = `perspective(1000px) translateX(${calcY}px) translateY(${calcX}px)`;
	}

	onMount(() => {
		ex1Layer = document.getElementById('ex1-layer') as HTMLElement;
		const container = document.getElementById('ex1') as HTMLElement;
		let isTouching = false;

		const touchStartHandler = (e: TouchEvent) => {
			isTouching = true;
			const touch = e.touches[0];
			transformElement(ex1Layer!, touch.clientX, touch.clientY);
			transformBackground(touch.clientX, touch.clientY);
		};

		const touchMoveHandler = (e: TouchEvent) => {
			if (isTouching) {
				const touch = e.touches[0];
				transformElement(ex1Layer!, touch.clientX, touch.clientY);
				transformBackground(touch.clientX, touch.clientY);
			}
		};

		const touchEndHandler = () => {
			isTouching = false;
			ex1Layer!.style.transform = 'perspective(1000px) rotateX(0deg) rotateY(0deg)';
		};

		container.addEventListener('touchstart', touchStartHandler);
		container.addEventListener('touchmove', touchMoveHandler);
		container.addEventListener('touchend', touchEndHandler);

		const mouseMoveHandler = (e: MouseEvent) => {
			const { clientX, clientY } = e;
			transformElement(ex1Layer!, clientX, clientY);
			transformBackground(clientX, clientY);
		};

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
						<span class="font-bold animated-color1 transition-colors duration-500">n</span><span
							class="font-bold animated-color2 transition-colors duration-500">e</span
						><span class="font-bold animated-color1 transition-colors duration-500">cr</span><span
							class="font-bold animated-color2 transition-colors duration-500">os</span
						><span class="font-bold animated-color1 transition-colors duration-500">sk</span><span
							class="font-bold animated-color2 transition-colors duration-500">ull</span
						>
					</div>
				{/each}
			</div>
			<div class="flex justify-center text-center space-x-2 mb-4 lg:text-2xl">
				{#each links as link}
					<a
						href={link.url}
						target="_blank"
						class="bg-purple-900 hover:bg-purple-400 hover:text-purple-950 hover:scale-110 transition-all duration-300 rounded-xl p-2 w-full font-bold"
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

	@keyframes colorAnimation1 {
		0%,
		100% {
			color: rgb(136, 20, 225); /* фиолетовый */
		}
		50% {
			color: rgb(252 231 243); /* розовый */
		}
	}

	@keyframes colorAnimation2 {
		0%,
		100% {
			color: #f4f4f4; /* белый */
		}
		50% {
			color: rgb(136, 20, 225); /* фиолетовый */
		}
	}

	.animated-color1 {
		animation: colorAnimation1 2s infinite;
	}

	.animated-color2 {
		animation: colorAnimation2 4s infinite;
	}
</style>
