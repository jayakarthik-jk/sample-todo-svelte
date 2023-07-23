<script lang="ts">
  import Li from "./components/Li.svelte";
  import Pagination from "./components/Pagination.svelte";

  let inputTodo = "";
  let todos = new Array<Todo>();
  let filteredTodos = todos;
  let idCount = 0;
  let sortOption: "asc" | "dsc" = "asc";
  let [completeOnly, inCompleteOnly] = [false, false];
  let isSearching = false;
  let currentPage = 1;
  const pageSize = 3;
  const createNewTodo = (text: string) => ({
    text,
    id: idCount++,
    isComplete: false,
  });
  const addTodo = () => {
    let newTodo = createNewTodo(inputTodo.trim());
    if (newTodo.text.length === 0) {
      // display the error
      return;
    }
    todos = [...todos, newTodo];
    inputTodo = "";
  };
  const deleteTodo = (id: number) => {
    todos = todos.filter((todo) => todo.id !== id);
  };
  const editTodo = (id: number) => {
    const todo = todos.find((todo) => todo.id === id);
    if (todo == null) {
      // display the error
      return;
    }
    inputTodo = todo.text;
    deleteTodo(todo.id);
  };
  const toggleComplete = (id: number) => {
    todos = todos.map((todo) =>
      todo.id === id ? { ...todo, isComplete: !todo.isComplete } : todo
    );
  };

  const toggleSortOption = () => {
    if (sortOption === "asc") {
      todos = todos.sort((a, b) => (a.text < b.text ? -1 : 0));
      sortOption = "dsc";
    } else {
      todos = todos.sort((a, b) => (a.text > b.text ? -1 : 0));
      sortOption = "asc";
    }
  };
  $: filteredTodos = todos;
  // filter
  $: if (completeOnly) {
    filteredTodos = filteredTodos.filter((todo) => todo.isComplete);
  } else if (inCompleteOnly) {
    filteredTodos = filteredTodos.filter((todo) => !todo.isComplete);
  } else {
    filteredTodos = todos;
  }
  // searching
  $: if (isSearching) {
    filteredTodos = todos.filter((todo) =>
      new RegExp(inputTodo, "i").test(todo.text)
    );
  }

  // pagination
  $: {
    const start = (currentPage - 1) * pageSize;
    const end = start + pageSize;
    filteredTodos = todos.slice(start, end);
  }
</script>

<main>
  <div class="input-container">
    <input
      type="text"
      bind:value={inputTodo}
      placeholder={isSearching ? "Search here" : "Enter todo to add"}
      class="inputTodo"
      on:keypress={(e) => e.key === "Enter" && !isSearching && addTodo()}
    />
    <button
      class="search-btn"
      style="background-color: {isSearching
        ? 'var(--primary)'
        : 'inherit'}; color: {isSearching ? 'white' : 'var(--primary)'}"
      on:click={() => (isSearching = !isSearching)}
    >
      <i class="fa-solid fa-magnifying-glass" />
    </button>
  </div>
  <div class="options-menu">
    <div>
      Filder by
      <label>
        <input
          type="checkbox"
          value="complete"
          name="filter"
          bind:checked={completeOnly}
        />
        <span>Completed</span>
      </label>
      <label>
        <input
          type="checkbox"
          value="not-completed"
          name="filter"
          bind:checked={inCompleteOnly}
        /><span> Not Completed </span>
      </label>
    </div>
    <button class="sort-by-btn" on:click={toggleSortOption}>
      Sort By
      {#if sortOption === "asc"}
        <i class="fa-solid fa-arrow-up" />
      {:else}
        <i class="fa-solid fa-arrow-down" />
      {/if}
    </button>
  </div>
  <div class="container">
    {#if todos.length === 0}
      <div class="no-todo-container">
        <span class="no-todo-text">No todos Found</span>
      </div>
    {:else}
      <ol>
        {#each filteredTodos as todo (todo.id)}
          <Li {todo} {deleteTodo} {editTodo} {toggleComplete} />
        {/each}
      </ol>
    {/if}
  </div>
  <Pagination
    {currentPage}
    totalPages={Math.ceil(todos.length / pageSize)}
    onPageChange={(page) => (currentPage = page)}
  />
</main>

<style>
  main {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    min-height: 100%;
    gap: 0.5rem;
  }
  .inputTodo {
    padding: 0.5rem 1rem;
    height: 2rem;
    text-align: center;
    font-size: large;
    border-radius: 0.5rem;
    border: 2px solid var(--primary);
    box-shadow: 5px 5px 25px #ccc;
    flex-grow: 1;
  }
  .inputTodo:focus {
    outline: none;
  }
  .container {
    height: 500px;
    width: 80%;
    padding: 1rem;
    border-radius: 0.5rem;
    border: 2px solid var(--primary);
    box-shadow: 5px 5px 25px #ccc;
    overflow-y: scroll;
  }
  .container::-webkit-scrollbar {
    display: none;
  }
  .no-todo-container {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .no-todo-text {
    font-size: larger;
    color: var(--primary);
  }
  .options-menu {
    width: 80%;
    padding: 0.5rem 1rem;
    font-size: large;
    color: var(--primary);
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: large;
  }
  .sort-by-btn {
    padding: 0.25rem;
    background-color: transparent;
    border: none;
    color: var(--primary);
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: large;
    cursor: pointer;
  }
  .input-container {
    width: 80%;
    display: flex;
    justify-content: space-between;
    gap: 1rem;
  }
  .search-btn {
    border: 2px solid var(--primary);
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 0.25rem 1rem;
    border-radius: 0.5rem;
    color: var(--primary);
    background: inherit;
    cursor: pointer;
    font-size: larger;
  }
</style>
