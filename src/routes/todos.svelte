<script lang="ts">
	import { onMount } from 'svelte';
	import { supabase } from '$lib/supabaseClient';
	import Todo from '$components/Todo.svelte';

	let todos: any = [];
	let newTask;
	let newDueDate;

	onMount(async () => {
		getAllTodos();
	});
	const getAllTodos = async () => {
		try {
			let { data, error } = await supabase
				.from('todos')
				.select('*')
				.order('dueDate', { ascending: true });
			todos = data;
		} catch (err) {
			console.log(err);
		}
	};

	//delete todo
	const deleteTodo = async (todo) => {
		try {
			let { data, error } = await supabase.from('todos').delete().eq('id', todo.id);
			await getAllTodos();
		} catch (error) {
			console.log(error);
		}
	};

	const addNewTodo = async (task) => {
		if (task !== '' || task !== undefined || task !== null) {
			try {
				const { data, error } = await supabase
					.from('todos')
					.insert([{ task: task, dueDate: newDueDate }]);
				await getAllTodos();
				newTask = '';
				newDueDate = '';
			} catch (error) {
				console.log(error);
			}
		}
	};
	const updateTodo = async (todo) => {
		try {
			const { data, error } = await supabase
				.from('todos')
				.update({ task: todo.task, isCompleted: todo.isCompleted, dueDate: todo.dueDate })
				.eq('id', todo.id);
			await getAllTodos();
		} catch (error) {
			console.log(error);
		}
	};
</script>

<div class="todos">
	{#each todos as todo}
		<Todo {todo} {updateTodo} {deleteTodo} />
	{:else}<p class="noTodos">You have no todos</p>{/each}
</div>
<div class="newTask">
	<input type="text" bind:value={newTask} />
	<input type="date" bind:value={newDueDate} />

	<button on:click={() => addNewTodo(newTask, newDueDate)}>Add Task</button>
</div>

<style>
	.noTodos {
		text-align: center;
		font-size: 1.25rem;
		grid-column: 1 / -1;
	}
	.todos {
		width: 100%;
		max-width: 500px;
		display: grid;
		grid-template-columns: 20px 1fr 1fr 1fr;
		gap: 10px;
	}
</style>
