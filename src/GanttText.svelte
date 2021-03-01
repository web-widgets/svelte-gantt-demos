<script>
	import { getData } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";
	import MyTaskContent from "./MyTaskContent.svelte";
	import MySidebarForm from "./MySidebarForm.svelte";

	export let skinSettings;

	const data = getData();
	data.tasks.forEach(a => {
		a.textRight = a.text;
		a.text = "";
	});

	data.columns[0].name = "textRight";
</script>

<Gantt
	{...$skinSettings}
	templates={{ taskText: MyTaskContent, sidebarForm: MySidebarForm }}
	tasks={data.tasks}
	links={data.links}
	scales={data.scales}
	columns={data.columns} />
