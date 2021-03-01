<script>
	import { format } from "date-fns";
	export let data;

	const mask = "yyyy.MM.dd";
	const type = data.type;
</script>

<div class="data">
	<div class="text"><span class="caption">{type}:</span> {data.text}</div>
	<div class="text">
		<span class="caption">start:</span>
		{format(data.start_date, mask)}
	</div>
	<div class="text">
		<span class="caption">end:</span>
		{format(data.end_date, mask)}
	</div>
</div>

<style>
	.data {
		padding: 10px;
		border-radius: 2px;
		box-shadow: 0px 0px 6px rgba(0, 0, 0, 0.25);
		font: var(--wx-small-font);
		color: var(--wx-font-color);
		background-color: #fefdfd;
		white-space: nowrap;
	}

	.text {
		font: var(--wx-tooltip-font);
		color: var(--wx-tooltip-font-color);
		text-transform: capitalize;
		margin-bottom: 5px;
	}

	.text:last-child {
		margin-bottom: 0;
	}

	.caption {
		font-weight: 700;
	}
</style>
