<!-- bookmark 20:10 -->

<script lang="ts">
	import TasksForm from './tasks-form.svelte';
	import TasksList from './tasks-list.svelte';
	import type { Filter, Task } from './types';

	let message = 'Tasks App';
	let currentFilter = $state<Filter>('all');
	let tasks = $state<Task[]>([]);
	let totalDone = $derived(tasks.filter((task) => task.done).length);
	let filteredTasks = $derived.by(() => {
		if (currentFilter === 'all') {
			return tasks;
		} else if (currentFilter === 'done') {
			return tasks.filter((task) => task.done);
		} else if (currentFilter === 'todo') {
			return tasks.filter((task) => !task.done);
		}

		return tasks;
	});

	$inspect(currentFilter);

	function addTask(newTask: string) {
		tasks.push({
			id: crypto.randomUUID(),
			title: newTask,
			done: false
		});
	}

	function toggleDone(task: Task) {
		task.done = !task.done;
	}

	function removeTask(id: string) {
		let index = tasks.findIndex((task) => task.id === id);
		tasks.splice(index, 1);
	}
</script>

{#snippet filterButton(filter: Filter)}
	<button
		class="secondary capitalized"
		class:contrast={currentFilter === filter}
		onclick={() => (currentFilter = filter)}>{filter}</button
	>
{/snippet}

<main>
	<h1>{message}</h1>
	<TasksForm {addTask} />
	<p>
		{#if tasks.length}
			{totalDone} / {tasks.length} tasks completed
		{:else}
			Add a task to get started
		{/if}
	</p>
	{#if tasks.length}
		<section class="button-container">
			{@render filterButton('all')}
			{@render filterButton('todo')}
			{@render filterButton('done')}
		</section>
	{/if}
	<TasksList tasks={filteredTasks} {toggleDone} {removeTask} />
</main>

<style>
	main {
		margin: 1rem auto;
		max-width: 800px;
	}

	.button-container {
		display: flex;
		justify-content: end;
		gap: 0.5rem;
	}

	.capitalized {
		text-transform: capitalize;
	}
</style>
