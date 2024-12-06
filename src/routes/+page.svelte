<!-- bookmark 2:12 -->

<script lang="ts">
	import TasksForm from './tasks-form.svelte';
	let message = 'Tasks App';
</script>

<main>
	<h1>{message}</h1>
	<TasksForm />
</main>

<style>
	main {
		margin: 1rem auto;
		max-width: 800px;
	}
</style>
