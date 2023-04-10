<script lang="ts">
	import { onMount } from 'svelte';
	import * as Tone from 'tone';

	let tone: Tone.Synth;
	let container: HTMLDivElement;

	export let octaves = [0, 1, 2, 3, 4, 5, 6, 7];
    export let markers: 'octaves' | 'notes' | 'none' = 'octaves';

	const notes = [
		['A0', 'A#0', 'B0'],
		['C1', 'C#1', 'D1', 'D#1', 'E1', 'F1', 'F#1', 'G1', 'G#1', 'A1', 'A#1', 'B1'],
		['C2', 'C#2', 'D2', 'D#2', 'E2', 'F2', 'F#2', 'G2', 'G#2', 'A2', 'A#2', 'B2'],
		['C3', 'C#3', 'D3', 'D#3', 'E3', 'F3', 'F#3', 'G3', 'G#3', 'A3', 'A#3', 'B3'],
		['C4', 'C#4', 'D4', 'D#4', 'E4', 'F4', 'F#4', 'G4', 'G#4', 'A4', 'A#4', 'B4'],
		['C5', 'C#5', 'D5', 'D#5', 'E5', 'F5', 'F#5', 'G5', 'G#5', 'A5', 'A#5', 'B5'],
		['C6', 'C#6', 'D6', 'D#6', 'E6', 'F6', 'F#6', 'G6', 'G#6', 'A6', 'A#6', 'B6'],
		['C7', 'C#7', 'D7', 'D#7', 'E7', 'F7', 'F#7', 'G7', 'G#7', 'A7', 'A#7', 'B7'],
		['C8']
	];

	$: chosenNotes = notes.slice(0, octaves.length).flat();
	$: nonSharpNotes = chosenNotes.filter((note) => !note.includes('#'));

	$: container && container.style.setProperty('--non-sharp-notes', nonSharpNotes.length.toString());

	onMount(async () => {
		tone = new Tone.Synth().toDestination();

		tone.oscillator.type = 'sine';

		await Tone.start();
	});
</script>

<div bind:this={container}>
	{#each chosenNotes as note}
		<button
			class={note.includes('#') ? 'black' : 'white'}
			on:click={() => {
				tone.triggerAttackRelease(note, '8n');
			}}
		>
            {#if markers == "notes"}
                {#if note.includes('#')}
                    <span>{note.replace('#', '♯').replace(/\d/, '')}</span><br />
                    <span>{note.slice(-1)}</span>
                {:else}
                    <span>{note}</span>
                {/if}
            {:else if markers == "octaves" && note.match(/C\d/)}
                <span>C{note.slice(-1)}</span>
            {/if}
		</button>
	{/each}
</div>

<style lang="scss">
	div {
		--non-sharp-notes: 51;
		--width: calc(100% / var(--non-sharp-notes));
	}

    span {
        position: absolute;
        bottom: 0;
        transform: translate(-50%, -50%);
    }

	button {
        position: relative;
		width: var(--width);
		height: 10rem;
		margin: 0;
		padding: 0;
        text-align: center;
		overflow-wrap: break-word;
        transition: all 0.1s ease-in-out;
	}

	button.white {
		box-shadow: -1px 0 0 rgba(255, 255, 255, 0.8) inset, 0 0 5px #ccc inset,
			0 0 3px rgba(0, 0, 0, 0.2);
		background: linear-gradient(to bottom, #eee 0%, #fff 100%);
		border-left: 0px solid #bbb;
		border-bottom: 1px solid #bbb;
		border-radius: 0 0 5px 5px;
	}

	button.black {
		background-color: black;
		color: white;
		height: 5rem;
		width: calc(var(--width) / 2);
		margin-left: calc(var(--width) / -2);
		margin-top: -10rem;
		transform: translateX(50%) translateY(-50%);
		border: 1px solid #000;
		border-radius: 0 0 3px 3px;
		box-shadow: -1px -1px 2px rgba(255, 255, 255, 0.2) inset,
			0 -5px 2px 3px rgba(0, 0, 0, 0.6) inset, 0 2px 4px rgba(0, 0, 0, 0.5);
		background: linear-gradient(45deg, #222 0%, #555 100%);
        z-index: 1;
	}

	.black:active {
		box-shadow: -1px -1px 2px rgba(255, 255, 255, 0.2) inset,
			0 -2px 2px 3px rgba(0, 0, 0, 0.6) inset, 0 1px 2px rgba(0, 0, 0, 0.5);
		background: linear-gradient(to right, #444 0%, #222 100%);
	}

	.white:active {
		box-shadow: 2px 0 3px rgba(0, 0, 0, 0.1) inset, -5px 5px 20px rgba(0, 0, 0, 0.2) inset,
			0 0 3px rgba(0, 0, 0, 0.2);
		background: linear-gradient(to bottom, #fff 0%, #e9e9e9 100%);
	}
</style>
