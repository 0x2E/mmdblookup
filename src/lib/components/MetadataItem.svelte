<script lang="ts">
	export let title = '';
	export let value: string | number = 'x';
	$: {
		if (!value) {
			value = 'x';
		}
	}
</script>

<div class="px-4 py-2 sm:grid sm:grid-cols-3 sm:gap-4 sm:px-0">
	<dt class="text-sm font-medium leading-6 break-words text-gray-900 dark:text-gray-200">
		{title}
	</dt>
	<dd
		class="mt-1 text-sm leading-6 break-words text-gray-700 sm:col-span-2 sm:mt-0 dark:text-gray-300"
	>
		{value}
	</dd>
</div>
