<script lang="ts">
	import Piano from '$lib/Piano.svelte';
</script>

<div>
	<Piano />
</div>

<style>
	/* move piano to bottom of page */
	div {
		position: absolute;
		bottom: 0;
		left: 0;
		right: 0;
	}
</style>
