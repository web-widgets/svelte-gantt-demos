<script>
	import Gantt from "@dhtmlx/trial-svelte-gantt";
	import { RestBackend } from "@dhtmlx/trial-lib-gantt";

	export let skinSettings;

	const url = "https://master--gantt-node--dev.webix.io";
	const server = new RestBackend(url);

	let tasks = [];
	let links = [];
	server.getData().then(data => {
		tasks = data.tasks;
		links = data.links;
	});
</script>

<Gantt
	{...$skinSettings}
	{tasks}
	{links}
	on:save={ev => server.saveData(ev.detail, server)} />
