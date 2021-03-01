<script>
	import { getData } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";

	export let skinSettings;

	const data = getData();
	const markers = [
		{
			start: new Date(2020, 2, 4),
			text: "Start Project",
			title: "Start point of project",
		},
		{
			start: new Date(2020, 2, 8),
			text: "Today",
			css: "myMiddleClass",
			title: "",
		},
		{
			start: new Date(2020, 2, 11),
			text: "End Project",
			css: "myEndClass",
			title: "End point of project",
		},
	];
</script>

<Gantt
	{...$skinSettings}
	{markers}
	tasks={data.tasks}
	links={data.links}
	scales={data.scales}
	columns={data.columns} />

<style>
	:global(.wx-default .myMiddleClass) {
		background-color: rgba(232, 49, 37, 0.77);
	}
	:global(.wx-material .myMiddleClass) {
		background-color: rgba(255, 84, 84, 0.77);
	}
	:global(.wx-default .myEndClass) {
		background-color: rgba(29, 180, 164, 0.77);
	}
	:global(.wx-material .myEndClass) {
		background-color: rgba(54, 206, 124, 0.77);
	}
</style>
