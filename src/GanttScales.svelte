<script>
	import { getData, complexScales } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";

	export let skinSettings;

	const data = getData();
</script>

<Gantt
	{...$skinSettings}
	tasks={data.tasks}
	links={data.links}
	scales={complexScales}
	columns={data.columns}
	start={new Date(2020, 2, 1)}
	end={new Date(2020, 3, 1)} />
