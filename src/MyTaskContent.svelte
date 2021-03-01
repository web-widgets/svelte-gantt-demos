<script>
	export let data;

	function addChild() {
		data.clicked = !data.clicked;
	}
</script>

<button on:click|stopPropagation={addChild}>
	{#if data.clicked}Was clicked{:else}Click Me{/if}
</button>

<style>
	button {
		font-size: 10px;
		position: relative;
		z-index: 2;
	}
</style>
