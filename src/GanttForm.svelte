<script>
	import { getData } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";
	import Form from "./Form.svelte";

	export let skinSettings;

	const data = getData();
	const taskTypes = ["task", "milestone"];

	let store;
	let task;

	function ganttAction(ev) {
		const { id, nId, action } = ev.detail;

		switch (action) {
			case "show-details":
				task = store.getTask(id);
				break;

			case "add-task":
				task = store.getTask(nId);
				break;

			case "close-form":
				task = null;
				break;

			default:
				break;
		}
	}

	function formAction(ev) {
		const { id, action, obj } = ev.detail;

		switch (action) {
			case "close-form":
				task = null;
				break;

			default:
				store.action(id, action, obj);
				break;
		}
	}
</script>

<div class="wrapper">
	<Gantt
		bind:store
		{...$skinSettings}
		readonly={{ noEdit: true }}
		tasks={data.tasks}
		links={data.links}
		scales={data.scales}
		columns={data.columns}
		start={new Date(2020, 2, 1)}
		end={new Date(2020, 3, 1)}
		on:action={ganttAction} />

	{#if task}
		<Form {task} {taskTypes} on:action={formAction} />
	{/if}
</div>

<style>
	.wrapper {
		height: 100%;
		overflow: hidden;
	}
</style>
