<script lang="ts">
	import { onMount } from 'svelte';
	import Piano from './Piano.svelte';
    import VisualizerRender from './VisualizerRender.svelte';
</script>

<div class="container">
    <div class="render">
        <VisualizerRender></VisualizerRender>
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