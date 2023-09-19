<script lang="ts">
	import { IconMenu2, IconMoon, IconSun, IconX } from '@tabler/icons-svelte';

	let darkMode = window.matchMedia('(prefers-color-scheme: dark)').matches;
	$: {
		let classList = document.documentElement.classList;
		if (darkMode) {
			classList.add('dark');
		} else {
			classList.remove('dark');
		}
	}

	let isNavbarOpen = false;

	const navLinks = [{ label: 'GitHub', link: 'https://github.com/0x2E/mmdblookup' }];
</script>

<nav
	class="flex flex-col md:flex-row items-center justify-between max-w-7xl w-full mx-auto mt-4 py-4 md:py-0 px-4 md:px-6 lg:px-8 bg-white border border-gray-200 rounded-[36px] dark:bg-gray-800 dark:border-gray-700"
>
	<div class="flex items-center justify-between w-full">
		<a
			class="flex items-center gap-2 text-xl font-semibold dark:text-white"
			href="/"
			aria-label="Brand"
		>
			<img class="w-8" src="/logo.png" alt="logo" />
			<span class="font-semibold dark:text-gray-200">mmdblookup</span>
		</a>
		<div class="md:hidden flex">
			<button
				class="p-2 rounded-full border bg-white shadow-sm align-middle hover:bg-gray-50 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-offset-white focus:ring-blue-600 transition-all text-sm dark:bg-slate-900 dark:hover:bg-slate-800 dark:border-gray-700 dark:focus:ring-offset-gray-800 dark:text-gray-200"
				aria-label="Toggle navigation"
				on:click={() => {
					isNavbarOpen = !isNavbarOpen;
				}}
			>
				{#if isNavbarOpen}
					<IconX class="w-4 h-4" />
				{:else}
					<IconMenu2 class="w-4 h-4" />
				{/if}
			</button>
		</div>
	</div>
	<div
		class="{!isNavbarOpen
			? 'hidden'
			: ''} md:flex md:flex-shrink-0 grow overflow-hidden transition-all duration-300"
	>
		<div
			class="flex flex-col md:flex-row gap-y-4 gap-x-0 mt-5 md:items-center md:justify-end md:gap-y-0 md:gap-x-4 md:mt-0"
		>
			{#each navLinks as link}
				<a
					class="font-medium text-gray-500 hover:text-gray-400 md:py-6 dark:text-gray-400 dark:hover:text-gray-500"
					href={link.link}>{link.label}</a
				>
			{/each}
			<button
				class="flex justify-center items-center text-black font-medium px-3 py-1.5 duration-300 transform rounded-lg dark:text-gray-200 hover:bg-gray-100 dark:hover:bg-gray-700"
				on:click={() => (darkMode = !darkMode)}
			>
				{#if darkMode}
					<IconSun />
				{:else}
					<IconMoon />
				{/if}
			</button>
		</div>
	</div>
</nav>
