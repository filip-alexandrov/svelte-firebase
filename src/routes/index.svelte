<script>
	let todos = [];

	let newTodo = '';

	const addTodo = () => {
		console.log(todos);
		todos = [...todos, { task: newTodo, isComplete: false, createdAt: new Date() }];
		newTodo = '';
	};

	const markTodoAsComplete = (index) => {
		todos[index].isComplete = !todos[index].isComplete;
		console.log(todos);
	};

	const deleteTodo = (index) => {
		let deleteItem = todos[index];
		todos = todos.filter((item) => item != deleteItem);
	};

	let error = '';
	const keyIsPressed = (event) => {
		if (event.key === 'Enter' && newTodo != '') {
			addTodo();
			error = '';
		} else if (event.key === 'Enter') {
			error = 'Todo is empty';
		}
	};
</script>

<input type="text" placeholder="Add a task" bind:value={newTodo} />
<button on:click={addTodo}>Add</button>

<ol>
	{#each todos as todo, index}
		<!-- if isComplete==true, class='complete' -->
		<li>
			<span class:complete={todo.isComplete}>
				{todo.task}
			</span>
			<span>
				<button on:click={() => markTodoAsComplete(index)}>✔</button>
				<button on:click={() => deleteTodo(index)}>❌</button>
			</span>
		</li>
	{:else}
		<p>No Todos</p>
	{/each}
	<p class="error">{error}</p>
</ol>

<svelte:window on:keydown={keyIsPressed} />

<style>
	.complete {
		text-decoration: line-through;
	}
	.error {
		color: red;
	}
</style>
