<script>
	let todos = [];

	let newTodo = '';

	const addTodo = () => {
		console.log(todos);
		todos = [...todos, { task: newTodo, isComplete: false, createdAt: new Date() }];
		newTodo = '';
	};
</script>

<input type="text" placeholder="Add a task" bind:value={newTodo} />
<button on:click={addTodo}>Add</button>

<ol>
	{#each todos as todo}
		<!-- if isComplete==true, class='complete' -->
		<li class:complete={todo.isComplete}>
			{todo.task}
		</li>
	{/each}
</ol>

<style>
	.complete {
		text-decoration: line-through;
	}
</style>
