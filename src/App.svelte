<script>
  import Icon from '@iconify/svelte';
  import { onMount } from 'svelte';

  let todos = [];
  let newTodo = '';
  let date = '';

  onMount(() => {
    date = new Date().toLocaleDateString();
  });

  function addTodo() {
    if (newTodo.trim()) {
      todos = [...todos, { text: newTodo.trim() }];
      newTodo = '';
    }
  }

  function confirmDelete(index) {
    if (confirm('Are you sure you want to delete this todo?')) {
      todos = todos.filter((_, i) => i !== index);
    }
  }

  function toggleComplete(index) {
    todos = todos.map((todo, i) => {
      if (i === index) {
        return { ...todo, completed: !todo.completed };
      }
      return todo;
    });
  }
</script>

<main class=" flex max-w-screen justify-center">
  <div class=" container border xl:w-[40%] md:w-[60%] lg:w-[50%] rounded-md shadow-sm p-2 md:p-4">
    <div class="flex items-center mb-2 justify-between">
      <div class="flex items-center">
        <img
          class=" w-[30px] md:w-[60px] h-[30px] md:h-[60px] object-cover"
          src="/sveltelogo.png"
          alt="logo"
        />
        <h1 class=" text-xl">Todo List</h1>
      </div>
      <h1 class=" font-bold">{date}</h1>
    </div>

    <div
      class="mb-5 flex border px-3 py-1 overflow-hidden items-center justify-between w-full rounded-full"
    >
      <input
        bind:value={newTodo}
        class=" outline-none border-none w-[80%] p-2 md:p-3"
        placeholder="Add a new todo"
        on:keyup={(e) => e.key === 'Enter' && addTodo()}
      />
      <button
        class=" py-1 md:py-2 hover:bg-orange-500 hover:border-orange-700 flex justify-center items-center hover:font-semibold hover:text-white duration-200 active:scale-95 px-3 rounded-full mb-[5px] w-[20%] mt-2"
        on:click={addTodo}
      >
        Add
      </button>
    </div>

    <ul>
      <h1 class=" font-semibold text-sm">Todo List:</h1>
      {#each todos as todo, index}
        <li
          class={`mb-2 p-3 w-full border border-orange-100 rounded-lg flex justify-between items-center   ${todo.completed ? 'completedParent' : ''} `}
        >
          <span class={`w-full font-semibold ${todo.completed ? 'completed' : ''}`}
            >{todo.text}</span
          >
          <div class="flex">
            <button
              class=" border-none ml-2"
              on:click={() => toggleComplete(index)}
            >
              {#if todo.completed}
			  <div class="p-2 border rounded-lg flex justify-center items-center hover:bg-orange-500 hover:border-orange-600 hover:text-white duration-200 text-orange-500" >
				  <Icon class=" border-none outline-none " height={20} icon="material-symbols:undo-rounded" />
			  </div>
              {:else}
			  <div class="p-2 border rounded-lg flex justify-center items-center hover:bg-orange-500 hover:border-orange-600 hover:text-white duration-200 text-orange-500"  >
				  <Icon class="border-none" height={20} icon="material-symbols:done" />
			  </div>
              {/if}
            </button>
            <button
              class=" ml-2 border-none "
              on:click={() => confirmDelete(index)}
            >
              <div class="p-2 border rounded-lg flex justify-center items-center hover:bg-orange-500 hover:border-orange-600 hover:text-white duration-200 text-orange-500">
				<Icon class="border-none " height={20} icon="material-symbols:delete-outline-rounded" />
			  </div>
            </button>
          </div>
        </li>
      {/each}
    </ul>
  </div>
</main>

<style>
  .completed {
    text-decoration: line-through;
    color: gray;
	
  }

  .completedParent {
	opacity: 0.3;
  }
  /* .iconn {
    padding: 12px;
    border: 1px solid black;
    border-radius: 10px;
  } */
</style>
