<script lang="ts">
	import { onMount } from 'svelte';
	import { Canvas, Layer, t, type Render } from 'svelte-canvas';

    let width: number = 640;
    let height: number = 640;

    let render: Render;
	$: render = ({ context, width, height }) => {
		context.fillStyle = `hsl(${$t / 40}, 100%, 50%)`;
		context.beginPath();
		context.arc(($t / 4) % width, ($t / 4) % height, 100, 0, Math.PI * 2);
		context.fill();
	};

    onMount(() => {
        window.addEventListener('resize', () => {
            width = window.innerWidth;
            height = window.innerHeight;
        });

        width = window.innerWidth;
        height = window.innerHeight;
    });
</script>

<Canvas {width} {height}>
	<Layer {render} />
</Canvas>
