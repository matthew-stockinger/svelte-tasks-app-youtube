<!-- bookmark 15:43 -->
 <!-- need to rewrite filteredTasks to $derived.by -->

<script lang="ts">
	import TasksForm from './tasks-form.svelte';
	import TasksList from './tasks-list.svelte';
	import type { Filter, Task } from './types';

	let message = 'Tasks App';
	let currentFilter = $state<Filter>('all');
	let tasks = $state<Task[]>([]);
	let totalDone = $derived(tasks.filter((task) => task.done).length);
	let filteredTasks = $derived(
		tasks.filter((task) => {
			if (currentFilter === 'all') {
				return true;
			} else if (currentFilter === 'done') {
				return task.done;
			} else {
				return !task.done;
			}
		})
	);

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

	function removeTask(index: number) {
		tasks.splice(index, 1);
	}
</script>

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
			<button class="secondary" onclick={() => (currentFilter = 'all')}
				>All</button
			>
			<button class="secondary" onclick={() => (currentFilter = 'todo')}
				>Todo</button
			>
			<button class="secondary" onclick={() => (currentFilter = 'done')}
				>Done</button
			>
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
</style>
