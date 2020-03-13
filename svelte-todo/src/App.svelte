<script>


	import { afterUpdate } from 'svelte';

	afterUpdate(() => {
		document.querySelector('.js-todo-input').focus();
	});

	let todoItems = [];
	let newTodo = '';

	function addTodo(){
		newTodo = newTodo.trim();
		if(!newTodo) return;

		const todo = {
			text: newTodo,
			checked: false,
			id: Date.now(),
		}
		todoItems = [...todoItems, todo];
		newTodo = '';		
	}

	function toggleDone(id){
		const index = todoItems.findIndex(item => item.id === Number(id));
		todoItems[index].checked = !todoItems[index].checked;		
	}

	function deleteTodo(id){
		todoItems = todoItems.filter(item => item.id !== Number(id));
	}
</script>

<main>
  <div>
    <h1>todos</h1>
    <ul class="todo-list">
			{#each todoItems as todo (todo.id)}			
			<li class="todo-item {todo.checked ? 'done' : ''}">
				<input id={todo.id} type="checkbox" on:click={() => toggleDone(todo.id)}/>
				<lable for={todo.id} class="tick" ></lable>
				<span>{todo.text}</span>
				<button class="delete-todo" on:click={() => deleteTodo(todo.id)}>
					Delete Item
				</button>
			</li>
			{/each}
		</ul>
    <div>      
      <h2>Add your first todo</h2>
      <p>What do you want to get done today?</p>
    </div>
    <form on:submit|preventDefault={addTodo}>
      <input bind:value={newTodo} class="js-todo-input" type="text" aria-label="Enter a new todo item" placeholder="E.g. Build a web app" />
    </form>
  </div>
</main>

<style>
	main {		
		padding: 1em;
		max-width: 350px;		
	}

	.done{
		background-color: #ff3e00;
		padding: 4px;
		border-radius: 3px;
	}

	.todo-list {
		text-align:left;
	}

	.delete-todo{
		text-align: right;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

</style>