<script>
	import { createEventDispatcher, onMount } from "svelte";
	import { getDiffer, getAdder } from "@dhtmlx/trial-lib-gantt";
	import Text from "@dhtmlx/trial-svelte-gantt/src/wx/Text.svelte";
	import Textarea from "@dhtmlx/trial-svelte-gantt/src/wx/Textarea.svelte";
	import Select from "@dhtmlx/trial-svelte-gantt/src/wx/Select.svelte";
	import Slider from "@dhtmlx/trial-svelte-gantt/src/wx/Slider.svelte";
	import DatePicker from "@dhtmlx/trial-svelte-gantt/src/wx/Datepicker.svelte";
	import Counter from "@dhtmlx/trial-svelte-gantt/src/wx/Counter.svelte";

	export let task = {};
	export let taskTypes;

	const dispatch = createEventDispatcher();

	let node, l, t, dx, dy, moving;

	let milestone = false;

	$: milestone = task.type === "milestone";

	onMount(() => {
		l = (window.innerWidth - node.offsetWidth) / 2;
		t = (window.innerHeight - node.offsetHeight) / 2;
	});

	function down(e) {
		const css = e.target.classList;
		if (css.contains("title")) {
			dx = e.pageX - node.offsetLeft;
			dy = e.pageY - node.offsetTop;
			moving = true;
		}
	}

	function move(e) {
		if (moving) {
			node.style.top = `${e.pageY - dy}px`;
			node.style.left = `${e.pageX - dx}px`;
		}
	}

	function up() {
		moving = false;
	}

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
			task.end_date = getAdder("day")(task.start_date, task.duration);
		} else {
			let duration = getDiffer("day")(task.end_date, task.start_date);
			if (duration > 0) task.duration = duration;
			else task.end_date = getAdder("day")(task.start_date, task.duration);
		}

		dispatch("action", { action: "update-task", id: task.id, obj: task });
	}
</script>

<div class="backdrop">
	<div
		class="modal"
		style="left:{l}px;top:{t}px"
		bind:this={node}
		on:mousedown={down}
		on:mousemove={move}
		on:mouseup={up}>
		<div class="header">
			<h3 class="title">Edit task</h3>
			<div class="icon-close" on:click={onClose}>&#9587;</div>
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

			<button class="btn btn-danger" on:click={deleteTask}>Delete</button>
		</div>
	</div>
</div>

<style>
	.backdrop {
		position: absolute;
		width: 100%;
		height: 100%;
		z-index: 5;
		background: rgba(0, 0, 0, 0.15);
	}

	.modal {
		position: absolute;
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

	.icon-close {
		position: absolute;
		top: 20px;
		right: 20px;
		width: 20px;
		height: 20px;
		cursor: pointer;
		font-weight: 700;
		transition: color 0.15s ease-in;
	}

	.icon-close:hover {
		color: rgb(255, 122, 122);
	}

	.body {
		margin: 20px 0;
	}

	.btn {
		padding: 8px 16px;
		border: none;
		border-radius: var(--wx-button-radius);
		font: var(--wx-font);
		font-weight: 500;
		text-transform: uppercase;
		color: #fff;
		cursor: pointer;
	}

	.btn:active {
		outline: none;
		opacity: 0.7;
	}

	.btn:focus {
		outline: none;
	}

	.btn-danger {
		color: var(--wx-danger-color);
		background-color: transparent;
	}

	.btn-danger:hover {
		background-color: var(--wx-danger-color-hover);
	}
</style>
