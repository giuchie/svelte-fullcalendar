<script>
	import { onMount, onDestroy } from 'svelte';
	import { Calendar } from '@fullcalendar/core';

	// General Props
	/** @type {string} */
	let classes = null;
	export { classes as class };
	/** @type {string} */
	export let style = null;

	/** @type {import('@fullcalendar/common').CalendarOptions}  */
	export let options;

	export function getAPI() {
		return calendar;
	}

	let calendarEl;
	let calendar;

	onMount(async () => {
		if (
			!options ||
			!options.plugins ||
			!options.plugins.length ||
			!calendarEl
		)
			return;

		initCalendar();
	});

	onDestroy(() => {
		if (calendar) calendar.destroy();
	});

	$: {
		if (
			options &&
			options.plugins &&
			options.plugins.length &&
			calendarEl &&
			!calendar
		)
			initCalendar();

		if (options && options.plugins && options.plugins.length && calendar)
			updateCalendarOptions();
	}

	function initCalendar() {
		calendar = new Calendar(calendarEl, options);
		calendar.render();
	}

	function updateCalendarOptions() {
		calendar.pauseRendering();
		calendar.resetOptions(options);
		calendar.resumeRendering();
	}
</script>

<div bind:this={calendarEl} class={classes} {style} />
