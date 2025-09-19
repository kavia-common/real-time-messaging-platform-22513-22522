<script lang="ts">
  // No props required for now
  function refresh() {
    // Use location.reload through a function bound via on:click (Svelte-compatible)
    location.reload();
  }
</script>

<div>
  <button class="button secondary" on:click={refresh} aria-label="Refresh">
    Refresh
  </button>
</div>
