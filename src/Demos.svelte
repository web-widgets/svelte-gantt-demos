<script>
	import DefaultTheme from "@dhtmlx/trial-svelte-gantt/src/wx/DefaultTheme.svelte";
	import MaterialTheme from "@dhtmlx/trial-svelte-gantt/src/wx/MaterialTheme.svelte";

	import Router, { link, push } from "svelte-spa-router";
	import active from "svelte-spa-router/active";
	import { wrap } from "svelte-spa-router/wrap";
	import { writable } from "svelte/store";

	import GanttMin from "./GanttMin.svelte";
	import GanttBackend from "./GanttBackend.svelte";
	import GanttScales from "./GanttScales.svelte";
	import GanttGrid from "./GanttGrid.svelte";
	import GanttNoGrid from "./GanttNoGrid.svelte";
	import GanttReadOnly from "./GanttReadOnly.svelte";
	import GanttForm from "./GanttForm.svelte";
	import GanttSizes from "./GanttSizes.svelte";
	import GanttMultiple from "./GanttMultiple.svelte";
	import GanttPerformance from "./GanttPerformance.svelte";
	import GanttMarkers from "./GanttMarkers.svelte";
	import GanttTooltips from "./GanttTooltips.svelte";
	import GanttText from "./GanttText.svelte";
	import GanttLocale from "./GanttLocale.svelte";

	const skins = [
		{
			id: "default",
			name: "Classic",
			props: { borders: "full", cellHeight: 38 },
		},
		{
			id: "material",
			name: "Material",
			props: { borders: "", cellHeight: 32 },
		},
	];

	let skin = null,
		page = null;
	let skinSettings = writable({});
	$: skinSettings.set((skins.find(a => a.id === skin) || {}).props);

	function toggleSkin(e) {
		const data = e.target.dataset;
		if (data.role === "skin") {
			push(`/${page}/${data.id}`);
		}
	}

	const demos = [
		["/base/:skin", "Gantt basic", GanttMin],
		["/backend/:skin", "Gantt with backend", GanttBackend],
		["/scales/:skin", "Custom scales", GanttScales],
		["/grid/:skin", "Custom grid", GanttGrid],
		["/form/:skin", "Custom edit form", GanttForm],
		["/chart/:skin", "Without the grid", GanttNoGrid],
		["/readonly/:skin", "Readonly mode", GanttReadOnly],
		["/sizes/:skin", "Scale / cell sizes", GanttSizes],
		["/many/:skin", "Many gantts per page", GanttMultiple],
		["/performance/:skin", "Performance", GanttPerformance],
		["/markers/:skin", "Markers", GanttMarkers],
		["/tooltips/:skin", "Tooltips", GanttTooltips],
		["/templates/:skin", "Custom text", GanttText],
		["/locale/:skin", "Locales", GanttLocale],
	];

	let title = "";
	const routes = {
		"/": wrap({
			component: {},
			conditions: () => {
				push("/base/default");
				return false;
			},
		}),
	};
	demos.forEach(
		a =>
			(routes[a[0]] = wrap({
				component: a[2],
				userData: a,
				props: { skinSettings },
				conditions: x => {
					const parts = x.location.split("/");
					page = parts[1];
					skin = parts[2];

					title = a[1];

					return true;
				},
			}))
	);

	let show = false;
	let noSidebar = document.location.search.indexOf("no-sidebar") !== -1;

	function onClick() {
		show = true;
	}
</script>

<DefaultTheme />
<MaterialTheme />

<div class="layout" class:no-sidebar={noSidebar}>
	<div class="sidebar" class:move={show} on:click={onClick}>
		{#if show}
			<div class="title">Gantt Demos</div>
		{/if}

		<div class="skins" class:move={!show} on:click|stopPropagation={toggleSkin}>
			{#each skins as data}
				<div
					class="skin"
					class:selected={data.id === skin}
					data-role="skin"
					data-id={data.id}>
					{data.name}
				</div>
			{/each}
		</div>

		{#if show}
			{#each demos as data, i}
				<a
					use:link={data[0].replace(':skin', skin)}
					use:active
					href="/"
					class="demo">
					{data[1]}
				</a>
			{/each}
		{:else}
			<div class="hint">{title}</div>
		{/if}
	</div>

	<div
		class="content wx-{skin}"
		class:move={show}
		on:click={() => (show = false)}>
		<Router {routes} />
	</div>
</div>

<style>
	:global(.sday) {
		font-weight: bold;
		background: rgba(235, 235, 235, 0.33);
	}

	.layout {
		position: relative;
		height: 100%;
		font-family: "Roboto", Arial, Helvetica, sans-serif;
		font-size: 16px;
		background-color: #ebebeb;
	}

	.sidebar {
		box-sizing: border-box;
		position: absolute;
		height: 100%;
		width: 300px;
		background-color: #fbfbfb;
		transform: translateX(-220px);
		transition: 0.3s;
		z-index: 1;
	}

	.sidebar.move {
		transform: translateX(0);
	}

	.content {
		height: 100%;
		width: calc(100% - 85px);
		transition: 0.3s;
		transform: translateX(85px);
	}

	.content.move {
		transform: translateX(305px);
		width: calc(100% - 305px);
	}

	.hint {
		position: absolute;
		top: 30px;
		right: 30px;
		font-size: 16px;
		font-weight: 500;
		color: #5f5f5f;
		transform: rotate(180deg);
		writing-mode: vertical-rl;
	}

	.title {
		height: 58px;
		line-height: 58px;
		margin-bottom: 30px;
		text-align: center;
		font-size: 16px;
		font-weight: 500;
		color: #5f5f5f;
		background-color: rgba(235, 235, 235, 0.61);
	}

	.skins {
		box-sizing: border-box;
		display: flex;
		justify-content: center;
		align-items: center;
		width: 250px;
		height: 30px;
		margin: 0 auto;
		margin-bottom: 30px;
		border-radius: 15px;
		text-align: center;
		background-color: #e6e6e6;
	}

	.skins.move {
		position: absolute;
		top: 330px;
		right: -86px;
		transform: rotate(-90deg);
	}

	.skin {
		box-sizing: border-box;
		width: 123px;
		height: 26px;
		line-height: 26px;
		border-radius: 13px;
		font-weight: 400;
		text-transform: capitalize;
		color: #1876d2;
		cursor: pointer;
	}

	.skin.selected {
		font-weight: 500;
		color: #fff;
		background-color: #2095f3;
	}

	.demo {
		height: 36px;
		line-height: 36px;
		padding-left: 24px;
		border-left: 5px solid transparent;
		color: #5f5f5f;
		list-style: none;
		cursor: pointer;
		text-decoration: none;
		display: block;
	}

	.demo.active,
	.demo:hover {
		border-left-color: #2095f3;
		background-color: #ebebeb9c;
		font-weight: 500;
		cursor: pointer;
	}

	.no-sidebar .sidebar {
		display: none;
	}
	.no-sidebar .content {
		width: 100%;
		transform: none;
	}
</style>
