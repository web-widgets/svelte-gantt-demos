<script>
	import { afterUpdate } from "svelte";
	import { getData, complexScales } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";
	import Button from "@dhtmlx/trial-svelte-gantt/src/wx/Button.svelte";

	export let skinSettings;

	const count = 10000;
	const years = 3;
	const data = getData("", count, 3);
	let start = null;
	let outArea;
	afterUpdate(() => {
		if (start && outArea) outArea.innerHTML = new Date() - start;
	});
</script>

<div class="rows">
	<div class="row">
		{#if start}
			10 000 tasks (
			{years}
			years ) rendered in
			<span bind:this={outArea} />
			ms
		{:else}
			<Button on:click={() => (start = new Date())}>Start</Button>
		{/if}
	</div>

	{#if start}
		<div class="gtcell">
			<Gantt
				{...$skinSettings}
				tasks={data.tasks}
				links={data.links}
				scales={complexScales}
				columns={data.columns} />
		</div>
	{/if}
</div>

<style>
	.rows {
		position: relative;
		display: flex;
		flex-direction: column;
		width: 100%;
		height: 100%;
		overflow: hidden;
	}

	.row {
		padding: 13px;
		font: var(--wx-font);
	}

	.gtcell {
		position: relative;
		height: 100%;
		min-height: 0;
		border-top: var(--wx-light-border);
		margin-bottom: 10px;
	}

	.gtcell:last-of-type {
		margin-bottom: 0;
	}
</style>
