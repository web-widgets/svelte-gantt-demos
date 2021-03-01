<script>
	import Text from "@dhtmlx/trial-svelte-gantt/src/wx/Text.svelte";
	import DatePicker from "@dhtmlx/trial-svelte-gantt/src/wx/Datepicker.svelte";
	import Links from "@dhtmlx/trial-svelte-gantt/src/components/sidebar/Links.svelte";

	export let task;
	export let linksData;
</script>

<Text autofocus={true} label="Task name" bind:value={task.textRight} />
<DatePicker label="Start date" bind:value={task.start_date} readonly />
<Links {linksData} on:action />
