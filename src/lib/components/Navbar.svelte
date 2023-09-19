<script lang="ts">
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
					<svg
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 24 24"
						stroke-width="1.5"
						stroke="currentColor"
						class="w-4 h-4"
					>
						<path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
					</svg>
				{:else}
					<svg
						xmlns="http://www.w3.org/2000/svg"
						fill="none"
						viewBox="0 0 24 24"
						stroke-width="1.5"
						stroke="currentColor"
						class="w-4 h-4"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5"
						/>
					</svg>
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
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 24 24"
						fill="currentColor"
						class="w-6 h-6"
					>
						<path
							d="M12 2.25a.75.75 0 01.75.75v2.25a.75.75 0 01-1.5 0V3a.75.75 0 01.75-.75zM7.5 12a4.5 4.5 0 119 0 4.5 4.5 0 01-9 0zM18.894 6.166a.75.75 0 00-1.06-1.06l-1.591 1.59a.75.75 0 101.06 1.061l1.591-1.59zM21.75 12a.75.75 0 01-.75.75h-2.25a.75.75 0 010-1.5H21a.75.75 0 01.75.75zM17.834 18.894a.75.75 0 001.06-1.06l-1.59-1.591a.75.75 0 10-1.061 1.06l1.59 1.591zM12 18a.75.75 0 01.75.75V21a.75.75 0 01-1.5 0v-2.25A.75.75 0 0112 18zM7.758 17.303a.75.75 0 00-1.061-1.06l-1.591 1.59a.75.75 0 001.06 1.061l1.591-1.59zM6 12a.75.75 0 01-.75.75H3a.75.75 0 010-1.5h2.25A.75.75 0 016 12zM6.697 7.757a.75.75 0 001.06-1.06l-1.59-1.591a.75.75 0 00-1.061 1.06l1.59 1.591z"
						/>
					</svg>
				{:else}
					<svg
						xmlns="http://www.w3.org/2000/svg"
						viewBox="0 0 24 24"
						fill="currentColor"
						class="w-6 h-6"
					>
						<path
							fill-rule="evenodd"
							d="M9.528 1.718a.75.75 0 01.162.819A8.97 8.97 0 009 6a9 9 0 009 9 8.97 8.97 0 003.463-.69.75.75 0 01.981.98 10.503 10.503 0 01-9.694 6.46c-5.799 0-10.5-4.701-10.5-10.5 0-4.368 2.667-8.112 6.46-9.694a.75.75 0 01.818.162z"
							clip-rule="evenodd"
						/>
					</svg>
				{/if}
			</button>
		</div>
	</div>
</nav>
