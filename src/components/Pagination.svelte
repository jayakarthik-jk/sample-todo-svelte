<script lang="ts">
  export let totalPages: number;
  export let currentPage: number;
  export let onPageChange: (page: number) => void;
  let pages: number[] = [];
  $: pages = Array.from({ length: totalPages }, (_, i) => i + 1);
</script>

{#if totalPages > 1}
  <ul class="pagination">
    <button
      class="page"
      disabled={currentPage === 1}
      on:click={() => onPageChange(currentPage - 1)}>&lt;</button
    >
    {#each pages as page (page)}
      <button
        class={`page ${page === currentPage ? "current" : ""}`}
        on:click={() => onPageChange(page)}
      >
        {page}
      </button>
    {/each}
    <button
      class="page"
      disabled={currentPage === totalPages}
      on:click={() => onPageChange(currentPage + 1)}>&gt;</button
    >
  </ul>
{/if}

<style>
  .pagination {
    display: flex;
    list-style: none;
    gap: 0.25rem;
  }
  .page {
    padding: 0.25rem 0.5rem;
    border-radius: 1rem;
    font-size: large;
    border: none;
    background: inherit;
    width: 2rem;
    height: 2rem;
  }
  .page:not(.current):hover {
    background: #e7e7e7;
  }
  .current {
    background-color: var(--primary);
    color: white;
  }
</style>
