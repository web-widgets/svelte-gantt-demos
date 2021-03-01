<script>
	import { getData, complexScales } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";
	import Button from "@dhtmlx/trial-svelte-gantt/src/wx/Button.svelte";

	export let skinSettings;

	let counter = 0;
	let gantts = [];
	addGantt();
	addGantt();

	function addGantt() {
		gantts = [
			...gantts,
			{
				id: counter,
				data: getData("[" + counter + "] "),
			},
		];
		counter++;
	}

	function removeGantt(id) {
		gantts = gantts.filter(a => a.id !== id);
	}
</script>

<div class="rows">
	<div class="row">
		<Button on:click={addGantt}>Add Gantt</Button>
	</div>

	{#each gantts as gantt}
		<div class="ganttCell">
			<div class="ganttHeader">
				<Button appearance="danger" on:click={() => removeGantt(gantt.id)}>
					Delete Gantt
				</Button>
			</div>
			<Gantt
				{...$skinSettings}
				tasks={gantt.data.tasks}
				links={gantt.data.links}
				scales={complexScales}
				columns={gantt.data.columns}
				start={new Date(2020, 2, 1)}
				end={new Date(2020, 3, 1)} />
		</div>
	{/each}
</div>

<style>
	.rows {
		display: flex;
		flex-direction: column;
		height: 100%;
	}

	.row {
		padding: 13px;
	}

	.ganttCell {
		height: 100%;
		width: 100%;
		display: flex;
		flex-direction: column;
		margin-bottom: 10px;
		overflow: hidden;
	}

	.ganttHeader {
		padding: 13px;
		background-color: #fff;
		border-bottom: 1px solid var(--wx-border-color);
	}
</style>
