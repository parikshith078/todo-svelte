<!-- YOU CAN DELETE EVERYTHING IN THIS PAGE -->
<script lang="ts">
	import type { TodoItem } from '$lib/types';
	import Todo from '$lib/components/Todo.svelte';

	let todoItems: TodoItem[] = [
		{ value: 'todo1', done: false },
		{ value: 'todo2', done: false },
		{ value: 'todo3', done: true }
	];
	let todoValue = '';

	const states = [
		{
			value: 'All',
			selected: true
		},
		{ value: 'Active', selected: false },
		{ value: 'Completed', selected: false }
	];

	const editTodo = (index: number, value: string) => {
		todoItems[index].value = value;
	};

	const add = () => {
		if (!todoValue || todoValue == '') return;
		todoItems = [...todoItems, { value: todoValue, done: false }];
		todoValue = '';
	};

	const deleteItem = (index: number) => {
		todoItems = todoItems.filter((_, i) => i != index);
	};

	const handleFilter = (state: number) => {
		states.forEach((state) => {
			state.selected = false;
		});
		states[state].selected = true;
	};

	$: numberOfItems = todoItems.filter((todo) => !todo.done).length;
	let displayItems: typeof todoItems;

	$: {
		if (states[0].selected) displayItems = todoItems;
		else if (states[1].selected) displayItems = todoItems.filter((todo) => !todo.done);
		else displayItems = todoItems.filter((todo) => todo.done);
	}
</script>

<div class="container h-full mx-auto flex justify-center items-center">
	<div class="space-y-5">
		<h1 class="h1">Todo List</h1>
		<p>Start by adding:</p>
		<div class="card p-4 .variant-glass-primary">
			<div class="add-todo flex gap-2 p-4">
				<input class="input" bind:value={todoValue} type="text" placeholder="Input" />
				<button on:click|preventDefault={add} class="btn variant-filled-primary">Add</button>
			</div>
			<div class=" p-4">
				<ol class="list p-2 text-xl">
					{#each displayItems as todo, index}
						<Todo {todo} {index} {editTodo} {deleteItem} />
					{/each}
					<!-- ... -->
				</ol>
				<div class="util flex items-center justify-around gap-6 py-2">
					<div>{numberOfItems} items left</div>
					<div class="flex">
						{#each states as s, i}
							<button
								on:click={() => handleFilter(i)}
								class={`btn ${s.selected && 'variant-ghost'}`}>{s.value}</button
							>
						{/each}
					</div>
					<button
						on:click={() => {
							todoItems = todoItems.filter((item) => !item.done);
						}}
						class="btn">Clear completed</button
					>
				</div>
			</div>
		</div>
	</div>
</div>
