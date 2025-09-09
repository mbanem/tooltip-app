<script lang="ts">
  export const ssr = false;
  import { onMount } from 'svelte';
  type Positions = {
    top: boolean;
    left: boolean;
    right: boolean;
    bottom: boolean;
  };
  let spaceTop = $state(0);

  // let delta = $derived(_scrollY());
  let spaceBottom = 0;
  let spaceLeft = 0;
  let spaceRight = 0;
  let tooltipWidth: number = 40;
  let tooltipHeight: number = 80;
  let el: HTMLButtonElement | null = null;
  const _random = () => {
    return window.scrollY;
  };
  let delta = $derived(80 * _random());
  function availablePositions(el: HTMLElement): Positions {
    const rect = el.getBoundingClientRect();

    // viewport dimensions
    const vw = window.innerWidth;
    const vh = window.innerHeight;

    // check available space around the element
    spaceTop = rect.top;
    spaceBottom = vh - rect.bottom;
    spaceLeft = rect.left;
    spaceRight = vw - rect.right;

    return {
      top: spaceTop >= tooltipHeight,
      bottom: spaceBottom >= tooltipHeight,
      left: spaceLeft >= tooltipWidth,
      right: spaceRight >= tooltipWidth,
    };
  }
  const showTooltip = () => {
    const { top, left, right, bottom } = availablePositions(el);
    console.log(top, left, right, bottom);
  };
  onMount(() => {
    // console.log(el)
  });
</script>

<button onmouseenter={showTooltip} bind:this={el}>hovering element</button>
<p style="margin:6rem 10rem;">
  spaceTop {spaceTop} tooltipHeight {tooltipHeight} delta {delta}
</p>
<div
  style={`position:absolute;top:12rem;left:${delta} (;width:120px;color:red;`}
>
  tooltip
</div>
<p style="color:red;margin-left:1rem;">
  scroll and tops space measures OK with true/false list OK
</p>
<div style="margin:60rem 80rem;color:blue;">making view</div>

<style>
  button {
    position: absolute;
    top: 10rem;
    left: 10rem;
  }
</style>
