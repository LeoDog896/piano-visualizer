<script lang="ts">
	import Piano from '$lib/Piano.svelte';
	import * as midiManager from 'midi-file';

	let files: FileList | undefined;
	let midi: midiManager.MidiData | undefined;

	function load(event: ProgressEvent<FileReader>) {
		if (!event.target) return;
		const array = event.target.result;
		if (!array) return;

		// files can either be a string or an array of bytes - coersion is needed
		const parsedArray =
			typeof array === 'string'
				? new Uint8Array([...array].map((c) => c.charCodeAt(0)))
				: new Uint8Array(array);
		midi = midiManager.parseMidi(parsedArray);
	}

	$: if (files && files.length > 0) {
		const reader = new FileReader();
		reader.addEventListener('load', load);
		reader.readAsArrayBuffer(files[0]);
	}
</script>

<Piano />

<br />

<input type="file" accept=".mid" bind:files />

{#if files}
	{#each files as file}
		<p>{file.name}</p>
	{/each}
{/if}

{#if midi}
	<p>{midi.header}</p>
{/if}
