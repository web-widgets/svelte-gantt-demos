<script>
	import { getData } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";
	import MyTooltipContent from "./MyTooltipContent.svelte";

	export let skinSettings;

	const data = getData();
</script>

<Gantt
	{...$skinSettings}
	tooltip={MyTooltipContent}
	tasks={data.tasks}
	links={data.links}
	scales={data.scales}
	columns={data.columns} />
