<script>
  let { children } = $props();
</script>

{@render children?.()}

<style>
  :global(body) {
    padding: 0;
    margin: 0;
  }
</style>
