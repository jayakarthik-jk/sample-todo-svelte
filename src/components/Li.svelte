<script lang="ts">
  export let todo: Todo;
  export let deleteTodo: (id: number) => void;
  export let editTodo: (id: number) => void;
  export let toggleComplete: (id: number) => void;
</script>

<li>
  <div>
    {todo.text}
    <div class="controls">
      <button
        class="icon is-complete-icon"
        on:click={() => toggleComplete(todo.id)}
      >
        {#if todo.isComplete}
          <i class="fa-solid fa-square-check" />
        {:else}
          <i class="fa-regular fa-square" />
        {/if}
      </button>
      <button class="icon edit-icon" on:click={() => editTodo(todo.id)}>
        <i class="fa-solid fa-pen-to-square" />
      </button>
      <button class="icon delete-icon" on:click={() => deleteTodo(todo.id)}>
        <i class="fa-solid fa-trash" />
      </button>
    </div>
  </div>
</li>

<style>
  li {
    border-bottom: 1px solid #ccc;
    padding: 0.75rem 0.5rem;
    font-size: 20px;
  }
  li > div {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .controls {
    display: flex;
    gap: 0 1rem;
  }
  .icon {
    padding: 0.25rem;
    background-color: white;
    border: none;
    cursor: pointer;
  }
  .is-complete-icon {
    color: darkseagreen;
  }
  .delete-icon {
    color: tomato;
  }
  .edit-icon {
    color: var(--primary);
  }
  i {
    font-size: x-large;
  }
</style>
