<script lang="ts">
	import type { Task } from './types';

	let {
		tasks,
		toggleDone
	}: { tasks: Task[]; toggleDone: (task: Task) => void } = $props();
</script>

<section>
	{#each tasks as task}
		<article>
			<label>
				<input
					type="checkbox"
					checked={task.done}
					onchange={() => toggleDone(task)}
				/>
				{task.title}
			</label>
		</article>
	{/each}
</section>
