<script lang="ts">
	import '../app.postcss';
	import Functions from './Functions.svelte';
</script>

<div class="app flex flex-col w-full items-center">
	<main class="">
		<slot />
		<Functions />
	</main>
</div>
