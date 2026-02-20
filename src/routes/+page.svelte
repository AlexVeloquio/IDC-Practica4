<script lang="ts">
	let showMessage = $state(false);
	let todos = $state<{ id: number; text: string; done: boolean }[]>([]);
	let newTodo = $state('');
	let nextId = $state(1);

	function addTodo() {
		if (newTodo.trim()) {
			todos.push({ id: nextId++, text: newTodo.trim(), done: false });
			newTodo = '';
		}
	}

	function removeTodo(id: number) {
		todos = todos.filter(todo => todo.id !== id);
	}

	function toggleDone(id: number) {
		todos = todos.map(todo => todo.id === id ? { ...todo, done: !todo.done } : todo);
	}
</script>

<div class="min-h-screen bg-gradient-to-br from-blue-500 to-purple-600 flex items-center justify-center">
	<div class="text-center text-white max-w-2xl mx-auto">
		<h1 class="text-4xl font-bold mb-4">Bienvenidos a la página de Svelte de Alejandro Rodríguez Veloquio</h1>
		<p class="text-lg mb-6">Explora esta aplicación interactiva con SvelteKit</p>
		<button
			class="bg-white text-blue-600 px-6 py-3 rounded-lg font-semibold hover:bg-gray-100 transition duration-300 mb-8"
			onclick={() => showMessage = !showMessage}
		>
			{showMessage ? 'Ocultar Mensaje' : 'Mostrar Mensaje Chido'}
		</button>
		{#if showMessage}
			<div class="mt-6 p-4 bg-white text-gray-800 rounded-lg shadow-lg mb-8">
				<p class="text-xl font-medium">¡Hola! Esta es una funcionalidad chida: un mensaje interactivo.</p>
				<p class="mt-2">Puedes hacer clic en el botón para mostrar u ocultar este mensaje. ¡Prueba más funcionalidades próximamente!</p>
			</div>
		{/if}

		<!-- Todo List -->
		<div class="bg-white text-gray-800 rounded-lg shadow-lg p-6">
			<h2 class="text-2xl font-bold mb-4">Lista de Tareas</h2>
			<div class="flex mb-4">
				<input
					bind:value={newTodo}
					onkeydown={(e) => e.key === 'Enter' && addTodo()}
					placeholder="Agregar nueva tarea..."
					class="flex-1 px-4 py-2 border border-gray-300 rounded-l-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
				/>
				<button
					onclick={addTodo}
					class="bg-blue-600 text-white px-4 py-2 rounded-r-lg hover:bg-blue-700 transition duration-300"
				>
					Agregar
				</button>
			</div>
			<ul class="space-y-2">
				{#each todos as todo (todo.id)}
					<li class="flex items-center justify-between p-2 bg-gray-50 rounded-lg">
						<label class="flex items-center">
							<input
								type="checkbox"
								checked={todo.done}
								onchange={() => toggleDone(todo.id)}
								class="mr-2"
							/>
							<span class={todo.done ? 'line-through text-gray-500' : ''}>{todo.text}</span>
						</label>
						<button
							onclick={() => removeTodo(todo.id)}
							class="text-red-500 hover:text-red-700 transition duration-300"
						>
							Eliminar
						</button>
					</li>
				{/each}
			</ul>
			{#if todos.length === 0}
				<p class="text-gray-500 mt-4">No hay tareas pendientes. ¡Agrega una!</p>
			{/if}
		</div>
	</div>
</div>
