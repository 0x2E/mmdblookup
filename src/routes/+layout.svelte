<script>
	import '../app.css';
	import Navbar from '$lib/components/Navbar.svelte';
	import Footer from '$lib/components/Footer.svelte';
</script>

<svelte:head>
	<title>mmdblookup</title>
</svelte:head>
<div class="min-h-screen flex flex-col bg-gray-50 dark:bg-gray-900">
	<Navbar />
	<main class="grow flex flex-col">
		<slot />
	</main>
	<Footer />
</div>
