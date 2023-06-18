<script lang="ts">
	import { onMount } from 'svelte';
	import { RotateCw, RotateCcw, FlipHorizontal2, FlipVertical2, XOctagon } from 'lucide-svelte';

	let canvas: HTMLCanvasElement;
	let context: CanvasRenderingContext2D;

	onMount(() => {
		context = canvas.getContext('2d') as CanvasRenderingContext2D;

		canvas.addEventListener('mousemove', handleMouseMove);
		canvas.addEventListener('mousedown', handleMouseDown);
		canvas.addEventListener('mouseup', handleMouseUp);
	});

	let isDrawing = false;
	let lastX = 0;
	let lastY = 0;

	function handleMouseMove(event: MouseEvent) {
		if (!isDrawing) return;
		context.beginPath();
		context.moveTo(lastX, lastY);
		context.lineTo(event.offsetX, event.offsetY);
		context.strokeStyle = '#000000';
		context.lineWidth = 10;
		context.stroke();
		[lastX, lastY] = [event.offsetX, event.offsetY];
	}

	function handleMouseDown(event: MouseEvent) {
		isDrawing = true;
		[lastX, lastY] = [event.offsetX, event.offsetY];
	}

	function handleMouseUp() {
		isDrawing = false;
	}

	let rotate = 0;
	let scaleX = 1;
	let scaleY = 1;

	function updateTransform() {
		canvas.style.transform = `rotate(${rotate}deg) scale(${scaleX}, ${scaleY})`;
	}

	function r(degrees: number) {
		rotate += degrees;
		updateTransform();
	}

	function flipX() {
		if (rotate % 180 !== 0) {
			scaleY *= -1;
			updateTransform();
			return;
		}
		scaleX *= -1;
		updateTransform();
	}

	function flipY() {
		if (rotate % 180 !== 0) {
			scaleX *= -1;
			updateTransform();
			return;
		}
		scaleY *= -1;
		updateTransform();
	}
</script>

<div class="grid grid-cols-[1fr_auto] grid-rows-[auto_1fr] w-screen h-screen">
	<h1 class="text-5xl font-extrabold col-span-2 p-10">
		<span class="text-blue-500">Flip</span>Draw
	</h1>
	<section class="flex justify-center items-center">
		<canvas
			width="500"
			height="500"
			class="bg-neutral-100 rounded-3xl transition duration-500 origin-center"
			bind:this={canvas}
		/>
	</section>
	<section class="flex flex-col pr-16 items-center justify-center gap-5">
		<button
			class="bg-neutral-100 p-5 rounded-2xl hover:bg-neutral-200 active:bg-neutral-300"
			on:click={() => r(90)}
		>
			<RotateCw size="80" />
		</button>
		<button
			class="bg-neutral-100 p-5 rounded-2xl hover:bg-neutral-200 active:bg-neutral-300"
			on:click={() => r(-90)}
		>
			<RotateCcw size="80" />
		</button>
		<button
			class="bg-neutral-100 p-5 rounded-2xl hover:bg-neutral-200 active:bg-neutral-300"
			on:click={flipX}
		>
			<FlipHorizontal2 size="80" />
		</button>
		<button
			class="bg-neutral-100 p-5 rounded-2xl hover:bg-neutral-200 active:bg-neutral-300"
			on:click={flipY}
		>
			<FlipVertical2 size="80" />
		</button>
		<button
			class="bg-neutral-100 p-5 rounded-2xl hover:bg-neutral-200 active:bg-neutral-300"
			on:click={() => context.clearRect(0, 0, canvas.width, canvas.height)}
		>
			<XOctagon size="80" />
		</button>
	</section>
</div>
