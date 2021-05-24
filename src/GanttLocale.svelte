<script>
	import { getData } from "./common/data";
	import Gantt from "@dhtmlx/trial-svelte-gantt";

	import Button from "@dhtmlx/trial-svelte-gantt/src/wx/Button.svelte";
	import RULocale from "@dhtmlx/trial-svelte-gantt/src/wx/RULocale.svelte";
	import CNLocale from "@dhtmlx/trial-svelte-gantt/src/wx/CNLocale.svelte";
	import ruGantt from "@dhtmlx/trial-svelte-gantt/src/locales/ru";
	import cnGantt from "@dhtmlx/trial-svelte-gantt/src/locales/cn";
	export let skinSettings;

	const data = getData();

	const langs = ["en", "ru", "cn"];
	let index = 0;
</script>

<div class="rows">
	<div style="padding:10px">
		<Button on:click={() => (index = index + 1)}>Change Locale</Button>
	</div>

	<div class="gtcell">
		{#if langs[index % 3] === 'en'}
			<Gantt
				{...$skinSettings}
				tasks={data.tasks}
				links={data.links}
				scales={data.scales}
				columns={data.columns} />
		{/if}

		{#if langs[index % 3] === 'ru'}
			<RULocale words={ruGantt}>
				<Gantt
					{...$skinSettings}
					tasks={data.tasks}
					links={data.links}
					scales={data.scales}
					columns={data.columns} />
			</RULocale>
		{/if}

		{#if langs[index % 3] === 'cn'}
			<CNLocale words={cnGantt}>
				<Gantt
					{...$skinSettings}
					tasks={data.tasks}
					links={data.links}
					scales={data.scales}
					columns={data.columns} />
			</CNLocale>
		{/if}
	</div>
</div>

<style>
	.rows {
		position: relative;
		display: flex;
		flex-direction: column;
		background-color: #fff;
		width: 100%;
		height: 100%;
		overflow: hidden;
	}
	.gtcell {
		position: relative;
		height: 100%;
		border-top: 1px solid var(--wx-border-color);
	}
</style>
