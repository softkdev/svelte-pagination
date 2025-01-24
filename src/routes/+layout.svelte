<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<main class="container mx-auto flex h-screen items-center justify-center px-10 py-10">
	{@render children()}
</main>
