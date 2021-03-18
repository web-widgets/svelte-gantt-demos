<script>
	import Gantt from "@dhtmlx/trial-svelte-gantt";
	import { RestDataProvider } from "@dhtmlx/gantt-data-provider";

	export let skinSettings;

	const url = "https://master--gantt-node--dev.webix.io";
	let store = null;

	const server = new RestDataProvider(url, {
		task: (id, obj) => store.updateTask(id, obj, true),
		link: (id, obj) => store.updateLink(id, obj, true),
	});

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
	bind:store
	on:save={ev => server.saveData(ev.detail)} />
