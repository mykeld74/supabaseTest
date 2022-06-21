<script>
	export let todo, updateTodo, deleteTodo;
</script>

<input
	type="checkbox"
	checked={todo.isCompleted}
	on:click={(e) => {
		todo.isCompleted = e.currentTarget.checked;
		updateTodo(todo);
	}}
/>
<input
	type="text"
	value={todo.task}
	on:input={(e) => {
		todo.task = e.currentTarget.value;
		updateTodo(todo);
	}}
	class:completed={todo.isCompleted}
/>
<input
	type="date"
	value={todo.dueDate}
	on:input={(e) => {
		todo.dueDate = e.currentTarget.value;
		updateTodo(todo);
	}}
/>
<button on:click={deleteTodo(todo)}>Delete</button>

<style>
	.completed {
		text-decoration: line-through;
	}
	input[type='text'] {
		border: none;
		border-bottom: 1px solid #777;
	}
</style>
