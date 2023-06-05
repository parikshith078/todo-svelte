<script lang="ts">
	import type { TodoItem } from '$lib/types';
	export let todo: TodoItem;
	export let index: number;
	export let deleteItem: (index: number) => void;
	export let editTodo: (index: number, value: string) => void;

	let isEditable = false;

	const toggleEdit = () => {
		isEditable = !isEditable;
	};
	function handleEdit(event: KeyboardEvent): void {
		let pressedKey = event.key;
		let targetElement = event.target as HTMLInputElement;
		let newTodo = targetElement.value;

		switch (pressedKey) {
			case 'Escape':
				targetElement.blur();
				break;
			case 'Enter':
				editTodo(index, newTodo);
				targetElement.blur();
				break;
		}
	}

	function handleBlur(
		event: FocusEvent & { currentTarget: EventTarget & HTMLParagraphElement }
	): any {
		let targetElement = event.target as HTMLInputElement;
		let newTodo = targetElement.value;

		editTodo(index, newTodo);
		targetElement.blur();
		toggleEdit();
	}
</script>

<li>
	<input type="checkbox" bind:checked={todo.done} />
	<p
		contenteditable={isEditable}
		on:dblclick={toggleEdit}
		on:keydown={(event) => handleEdit(event)}
		on:blur={(event) => handleBlur(event)}
		class="flex-auto"
	>
		{todo.value}
	</p>
	<button
		type="button"
		on:click|preventDefault={() => deleteItem(index)}
		class="btn-icon hover:scale-105"
	>
		<svg
			xmlns="http://www.w3.org/2000/svg"
			class="icon icon-tabler icon-tabler-x"
			width="24"
			height="24"
			viewBox="0 0 24 24"
			stroke-width="1.5"
			stroke="#fff"
			fill="none"
			stroke-linecap="round"
			stroke-linejoin="round"
		>
			<path stroke="none" d="M0 0h24v24H0z" fill="none" />
			<line x1="18" y1="6" x2="6" y2="18" />
			<line x1="6" y1="6" x2="18" y2="18" />
		</svg>
	</button>
</li>
