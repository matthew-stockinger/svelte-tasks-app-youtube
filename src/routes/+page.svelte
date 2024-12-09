<!-- bookmark 12:12 -->

<script lang="ts">
	import TasksForm from './tasks-form.svelte';
	import TasksList from './tasks-list.svelte';
	import type { Task } from './types';

	let message = 'Tasks App';
	let tasks = $state<Task[]>([]);
	let totalDone = $derived(tasks.filter((task) => task.done).length);

	$inspect(tasks);

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
</script>

<main>
	<h1>{message}</h1>
	<TasksForm {addTask} />
	<p>{totalDone} / {tasks.length} tasks completed</p>
	<TasksList {tasks} {toggleDone} />
</main>

<style>
	main {
		margin: 1rem auto;
		max-width: 800px;
	}
</style>
