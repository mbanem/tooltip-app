<script lang="ts">
  import favicon from '$lib/assets/favicon.svg';

  let { children } = $props();
</script>

<svelte:head>
  <link rel="icon" href={favicon} />
</svelte:head>
<div class="toolbar">
  <a href="/">Home</a>
  <a href="/around">Around</a>
  <a href="/grok">Grok Tooltip</a>
  <a href="/random-position">Random Position</a>
  <a href="/sample">Sample</a>
  <a href="/tooltip">Tooltip</a>
  <a href="/tt">TT</a>
  <a href="/xx">XX</a>
  <a href="/zoom-image">Zoom Image</a>
  <a href="/tt-with-args">With arguments</a>
</div>
{@render children?.()}

<style>
  .toolbar {
    display: flex;
    gap: 1rem;
    a {
      text-decoration: none;
      color: navy;
    }
  }
</style>
