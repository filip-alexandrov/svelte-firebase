<script>
	import { initializeApp, getApps, getApp } from 'firebase/app';
	import { getFirestore } from 'firebase/firestore';
	import { firebaseConfig } from '$lib/firebaseConfig';
	import { browser } from '$app/env';
	import { collection, onSnapshot } from 'firebase/firestore';

	let firebaseApp;
	let db;
	let colRef;
	if (browser) {
		if (getApps().length === 0) {
			firebaseApp = initializeApp(firebaseConfig);
		} else {
			getApp();
		}
		db = getFirestore();
		colRef = collection(db, 'todos');
	}

	let todos = [];
	const unsubscribe =
		browser &&
		onSnapshot(colRef, (querySnapshot) => {
			let firebaseTodos = [];
			querySnapshot.forEach((doc) => {
				let todo = { ...doc.data(), id: doc.id };
				firebaseTodos = [todo, ...firebaseTodos];
			});
			todos = firebaseTodos;
		});

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
