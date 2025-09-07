<script lang="ts">
  import { onMount } from 'svelte';
  type Positions = {
    top: boolean;
    left: boolean;
    right: boolean;
    bottom: boolean;
  };
  let hoveringElement: HTMLButtonElement | null = null;
  let spaceTop = $state(0);
  let spaceBottom = $state(0);
  let spaceLeft = $state(0);
  let spaceRight = $state(0);

  const hoverRect = () => {
    return hoveringElement?.getBoundingClientRect() as DOMRect;
  };
  let hr = $derived(hoverRect());
  console.log('hoverRect', hr, 'tooltipRect');
  function availablePositions(
    hoveringElement: HTMLElement,
    tooltipWidth: number = 140,
    tooltipHeight: number = 80,
  ): Positions {
    const hoveringRect = hoveringElement.getBoundingClientRect();

    // console.log(
    //   hoveringRect.top,
    //   hoveringRect.left,
    //   hoveringRect.right,
    //   hoveringRect.bottom,
    // );
    // viewport dimensions
    const vw = window.innerWidth;
    const vh = window.innerHeight;

    // check available space around the element
    spaceTop = hoveringRect.top;
    spaceBottom = vh - hoveringRect.bottom;
    spaceLeft = hoveringRect.left;
    spaceRight = vw - hoveringRect.right;

    // console.log(
    //   'space top',
    //   spaceTop - tooltipHeight,
    //   'space left',
    //   spaceLeft - tooltipWidth,
    // );
    return {
      top: spaceTop >= tooltipHeight,
      bottom: spaceBottom >= tooltipHeight,
      left: spaceLeft >= tooltipWidth,
      right: spaceRight >= tooltipWidth,
    };
  }
  const showTooltip = () => {
    console.log(
      'hoveringRect',
      hoveringElement?.getBoundingClientRect() as DOMRect,
    );
    const { top, left, right, bottom } = availablePositions(
      hoveringElement as HTMLElement,
    );
    console.log(top, left, right, bottom);
  };
  onMount(() => {
    console.log(hoveringElement);
  });
</script>

<a href="/">Home</a>
<p style="color:blue;margin-left:1rem;">
  Using scroll Positions as true/false list and DOMRects
</p>

<!-- <p>
  hover rect {hr.top}, hr.left, {hr.left}
  hoverRect.right, {hr.right}
  hoverRect.bottom {hr.bottom}
</p> -->
<button onmouseenter={showTooltip} bind:this={hoveringElement}
  >hovering element</button
>
<div style="margin:60rem 80rem;">making view</div>

<style>
  button {
    position: absolute;
    top: 10rem;
    left: 10rem;
  }
</style>
