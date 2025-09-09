<script lang="ts">
  import { onMount } from 'svelte';
  // import { scrollToPosition } from '$lib/utils/helpers';
  import { afterNavigate } from '$app/navigation';

  const scrollToPosition = (top: number, left: number) => {
    afterNavigate(() => {
      window.scroll({ top, left, behavior: 'smooth' });
    });
  };
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
  let okPos = $state<string>('');

  const hoverRect = () => {
    return hoveringElement?.getBoundingClientRect() as DOMRect;
  };
  let hr = $derived(hoverRect());
  // console.log('hoverRect', hr, 'tooltipRect');
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
    // console.log(
    //   'hoveringRect',
    //   hoveringElement?.getBoundingClientRect() as DOMRect,
    // );
    // okPos = availablePositions(hoveringElement as HTMLElement);
    console.log(okPos);
  };

  onMount(() => {
    scrollToPosition(450, 100);
    // console.log(hoveringElement);
    // okPos = availablePositions(hoveringElement as HTMLElement);
    const color = (b: boolean) => {
      return `<span style="color:${b ? 'green' : 'red'}">${String(b).toUpperCase()}</span>`;
    };
    addEventListener('scroll', (event) => {
      const ap = availablePositions(hoveringElement as HTMLElement);
      okPos = `top ${color(ap.top)}, left ${color(ap.left)}, right ${color(ap.right)}, bottom ${color(ap.bottom)}`;
    });
  });
</script>

<p style="color:blue;margin-left:1rem;">
  Using scroll Positions as true/false list and DOMRects
</p>

<!-- <p>
  hover rect {hr.top}, hr.left, {hr.left}
  hoverRect.right, {hr.right}
  hoverRect.bottom {hr.bottom}
</p> -->
<div
  style="display:flex;flex-direction:column;width:40rem;margin:55rem 70rem 0 60rem;"
>
  <div style="user-select:none;">{@html okPos}</div>
  <button
    onmouseenter={showTooltip}
    bind:this={hoveringElement}
    style="height:3rem;"
    >scroll to see available space for tooltip around</button
  >
</div>
<div style="margin:120rem 250rem 140rem 160rem !important;">making view</div>

<style>
</style>
