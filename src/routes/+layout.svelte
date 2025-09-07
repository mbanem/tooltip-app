<script lang="ts">
  import favicon from '$lib/assets/favicon.svg';

  let { children } = $props();
</script>

<svelte:head>
  <link rel="icon" href={favicon} />
</svelte:head>
<a href="/">Home</a>
<a href="/around">Around</a>
<a href="/grok">Grok Tooltip</a>
<a href="/random-position">Random Position</a>
<a href="/sample">Sample</a>
<a href="/tooltip">Tooltip</a>
<a href="/tt">TT</a>
<a href="/xx">XX</a>
<a href="/tt-with-args">With arguments</a>
<a href="/zoom-image">Zoom Image</a>
{@render children?.()}
