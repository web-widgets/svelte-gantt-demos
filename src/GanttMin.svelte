<script>
	import { getData } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";

	export let skinSettings;

	const data = getData();
</script>

<Gantt
	{...$skinSettings}
	tasks={data.tasks}
	links={data.links}
	scales={data.scales}
	columns={data.columns} />
