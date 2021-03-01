<script>
	import { getData, complexScales } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";
	import Slider from "@dhtmlx/trial-svelte-gantt/src/wx/Slider.svelte";

	export let skinSettings;

	const data = getData();

	let cellWidth = 100;
	let scaleHeight = 38;
	let cellHeight = $skinSettings.cellHeight;
</script>

<div class="rows">
	<div style="padding:10px">
		<Slider label="Cell width" bind:value={cellWidth} min={20} max={200} />
		<Slider label="Cell height" bind:value={cellHeight} min={20} max={60} />
		<Slider label="Scale height" bind:value={scaleHeight} min={20} max={60} />
	</div>

	<div class="gtcell">
		<Gantt
			{...$skinSettings}
			tasks={data.tasks}
			links={data.links}
			scales={complexScales}
			columns={data.columns}
			{cellWidth}
			{cellHeight}
			{scaleHeight}
			start={new Date(2020, 2, 1)}
			end={new Date(2020, 3, 1)} />
	</div>
</div>

<style>
	.rows {
		position: relative;
		display: flex;
		flex-direction: column;
		background-color: #fff;
		width: 100%;
		height: 100%;
		overflow: hidden;
	}
	.gtcell {
		position: relative;
		height: 100%;
		border-top: 1px solid var(--wx-border-color);
	}
</style>
