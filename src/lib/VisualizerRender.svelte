<script lang="ts">
	import { onMount } from 'svelte';
	import { Canvas, Layer, t, type Render } from 'svelte-canvas';
    import { toPosition } from './notes';

    const startTime = 1000;

    let width: number = 640;
    let height: number = 640;
    let container: HTMLDivElement;

    type TimeRange = [begin: number, end: number | undefined]

    let notes: [name: string, time: TimeRange][] = []

    export function startPlayingNote(note: string) {
        notes.push([note, [startTime, undefined]]);
    }

    export function stopPlayingNote(note: string) {
        const foundNote = notes.findIndex(n => n[0] === note && n[1][1] === undefined);

        if (foundNote !== -1) {
            notes[foundNote][1][1] = startTime;
        }
    }

    let render: Render;
	$: render = ({ context, width, height }) => {
		$t;

        notes = notes.map(([name, [timeStart, timeEnd]], i) => {
            const [startX, endX] = toPosition(name).map(x => x * width);

            const [fromY, toY] = [timeStart, timeEnd].map(time => {
                if (time === undefined) {
                    return 0;
                }

                return height - (time / startTime) * height;
            });

            context.beginPath();
            context.rect(startX, fromY, endX - startX, toY - fromY);
            context.fillStyle = 'red';
            context.fill();

            return [name, [timeStart - 1, timeEnd === undefined ? undefined : timeEnd - 1]];
        });
	};

    onMount(() => {
        window.addEventListener('resize', () => {
            width = container.clientWidth;
            height = container.clientHeight;
        });

        width = container.clientWidth;
        height = container.clientHeight;
    });
</script>

<div bind:this={container}>
    <Canvas {width} {height}>
        <Layer {render} />
    </Canvas>    
</div>

<style>
    div {
        width: 100%;
        height: 100%;
    }
</style>