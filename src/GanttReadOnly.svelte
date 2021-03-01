<script>
	import { getData } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";

	export let skinSettings;

	const data = getData();

	const noDrag = false; // if true - can't move and dnd tasks
	const noEdit = false; // if true - can't edit task
	const noNewLink = false; // if true - can't create new links

	const readonly =
		noDrag || noEdit || noNewLink ? { noDrag, noEdit, noNewLink } : true;
</script>

<Gantt
	{readonly}
	{...$skinSettings}
	tasks={data.tasks}
	links={data.links}
	scales={data.scales}
	columns={data.columns.filter(a => a.name != 'add-task')}
	start={new Date(2020, 2, 1)}
	end={new Date(2020, 3, 1)} />
