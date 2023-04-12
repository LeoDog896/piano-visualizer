<script lang="ts">
	import { onMount } from 'svelte';
	import Piano from './Piano.svelte';
    import VisualizerRender from './VisualizerRender.svelte';

    let renderer: VisualizerRender;

    onMount(() => {
        setInterval(() => {
            const note = "C" + Math.floor(Math.random() * 8);
            renderer.startPlayingNote(note);
            setTimeout(() => {
                renderer.stopPlayingNote(note);
            }, 500);
        }, 2000);
    })
</script>

<div class="container">
    <div class="render">
        <VisualizerRender bind:this={renderer}></VisualizerRender>
    </div>
    <div class="piano">
        <Piano></Piano>
    </div>
</div>

<style>
    .container {
        display: flex;
        flex-direction: column;
        height: 100%;
        width: 100%;
    }

    .render {
        flex: 1;
    }

    .piano {
        flex: 0;
    }
</style>