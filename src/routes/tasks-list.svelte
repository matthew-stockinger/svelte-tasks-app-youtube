<script lang="ts">
	import type { Task } from './types';

	let {
		tasks,
		toggleDone
	}: { tasks: Task[]; toggleDone: (task: Task) => void } = $props();
</script>

<section>
	{#each tasks as task}
		<article class="task">
			<label>
				<input
					type="checkbox"
					checked={task.done}
					onchange={() => toggleDone(task)}
				/>
				<span class:done={task.done}>{task.title}</span>
			</label>
			<button class="outline">Remove</button>
		</article>
	{/each}
</section>

<style>
	.done {
		text-decoration: line-through;
	}

	.task {
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
</style>
