<script>
	import { createEventDispatcher, onMount } from "svelte";
	import { differenceInCalendarDays, addDays } from "date-fns";
	import Text from "@dhtmlx/trial-svelte-gantt/src/wx/Text.svelte";
	import Textarea from "@dhtmlx/trial-svelte-gantt/src/wx/Textarea.svelte";
	import Select from "@dhtmlx/trial-svelte-gantt/src/wx/Select.svelte";
	import Slider from "@dhtmlx/trial-svelte-gantt/src/wx/Slider.svelte";
	import DatePicker from "@dhtmlx/trial-svelte-gantt/src/wx/Datepicker.svelte";
	import Counter from "@dhtmlx/trial-svelte-gantt/src/wx/Counter.svelte";

	export let task = {};
	export let taskTypes;

	const dispatch = createEventDispatcher();

	let node, l, t;

	let milestone = false;

	$: milestone = task.type === "milestone";

	onMount(() => {
		l = (window.innerWidth - node.offsetWidth) / 2;
		t = (window.innerHeight - node.offsetHeight) / 2;
	});

	function deleteTask() {
		dispatch("action", { action: "delete-task", id: task.id });
		dispatch("action", { action: "close-form" });
	}

	function onClose() {
		dispatch("action", { action: "close-form" });
	}

	function onDurationChange(e) {
		task.duration = e.detail.value * 1;
		task.end_date = null;
	}

	$: {
		if (!task.end_date) {
			task.end_date = addDays(task.start_date, task.duration);
		} else {
			let duration = differenceInCalendarDays(task.end_date, task.start_date);
			if (duration > 0) task.duration = duration;
			else task.end_date = addDays(task.start_date, task.duration);
		}

		dispatch("action", { action: "update-task", id: task.id, obj: task });
	}
</script>

<div class="backdrop">
	<div class="modal" style="left:{l}px;top:{t}px" bind:this={node}>
		<div class="header">
			<h3 class="title">Edit task</h3>
			<div class="close" on:click={onClose}>&#9587;</div>
		</div>
		<div class="body">
			<Text autofocus={true} label="Name" bind:value={task.text} />

			<Textarea label="Description" bind:value={task.details} />

			{#if taskTypes.length > 1}
				<Select label="Type" bind:value={task.type} options={taskTypes} />
			{/if}

			<DatePicker label="Start date" bind:value={task.start_date} readonly />

			{#if !milestone}
				<DatePicker label="End date" bind:value={task.end_date} readonly />
				<Counter
					label="Duration"
					bind:value={task.duration}
					min={1}
					max={100}
					on:change={onDurationChange} />

				<Slider label="Progress: {task.progress}%" bind:value={task.progress} />
			{/if}

			<button class="button danger" on:click={deleteTask}>Delete</button>
		</div>
	</div>
</div>

<style>
	.backdrop {
		position: fixed;
		top: 0;
		left: 0;
		bottom: 0;
		right: 0;
		z-index: 5;
		background: rgba(0, 0, 0, 0.15);
	}

	.modal {
		position: relative;
		width: 340px;
		padding: 20px;
		border-radius: 6px;
		box-shadow: 0 4px 4px rgba(0, 0, 0, 0.12), 0 0 10px rgba(0, 0, 0, 0.06);
		background-color: #fff;
		font: var(--wx-font);
		color: var(--wx-font-color);
	}

	.header {
		cursor: move;
	}

	.title {
		margin: 0;
	}

	.close {
		position: absolute;
		top: 20px;
		right: 20px;
		width: 20px;
		height: 20px;
		cursor: pointer;
		font-weight: 700;
		transition: color 0.15s ease-in;
	}

	.close:hover {
		color: rgb(255, 122, 122);
	}

	.body {
		margin: 20px 0;
	}

	.button {
		padding: 10px;
		margin: 0 0 0.5em 0;
		box-sizing: border-box;
		border: 1px solid #ccc;
		border-radius: 2px;
		font: var(--wx-font);
		border-radius: 3px;
		cursor: pointer;
	}

	.button:focus {
		outline: none;
		opacity: 0.7;
	}

	.danger {
		background-color: var(--wx-danger-color);
		color: var(--wx-inverted-color);
	}
</style>
