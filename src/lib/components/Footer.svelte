<script lang="ts">
	import { IconBrandGithubFilled } from '@tabler/icons-svelte';

	const iconList: { icon: any; link: string; label: string }[] = [
		{ icon: IconBrandGithubFilled, link: 'https://github.com/0x2E/mmdblookup', label: 'GitHub' }
	];
</script>

<footer class="px-4 py-6 bg-gray-100 dark:bg-gray-800">
	<div class="max-w-screen-xl mx-auto md:flex md:items-center md:justify-between">
		<span class="text-sm text-gray-500 dark:text-gray-400 sm:text-center">
			<a target="_blank" href="https://icons8.com/icon/3nOZtpH7KQrP/location">Location</a> icon by
			<a target="_blank" href="https://icons8.com">Icons8</a>
		</span>
		<div class="flex mt-4 space-x-5 sm:justify-center md:mt-0">
			{#each iconList as icon}
				<a href={icon.link} class="text-gray-400 hover:text-gray-900 dark:hover:text-white">
					<svelte:component this={icon.icon} class="w-4 h-4" />
					<span class="sr-only">{icon.label}</span>
				</a>
			{/each}
		</div>
	</div>
</footer>
