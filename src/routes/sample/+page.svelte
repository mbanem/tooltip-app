<script lang="ts">
  import { onMount, type Snippet } from 'svelte';
  // negative property indicates  to position is unavailable
  type Positions = {
    top: number;
    left: number;
    right: number;
    bottom: number;
  };
  let positions: Positions = {
    top: 0,
    left: 0,
    right: 0,
    bottom: 0,
  };
  const roundRectProps = (obj: HTMLElement) => {
    let rect = obj.getBoundingClientRect() as DOMRect;
    const { top, left, width, height } = rect;
    console.log('roundRectProps', top, left, width, height);
    // console.log(rect)
    // Object.entries(rect).forEach(([key, value]) => {
    //   rect[key] = Number.parseFloat(value).toFixed(2);
    // });

    rect = Object.fromEntries(
      Object.entries(rect).map(([key, value]) => [
        key,
        typeof value === 'number' ? Number.parseFloat(value.toFixed(2)) : value,
      ]),
    );
    console.log(rect);
    return rect;
  };
  let tooltipRect: DOMRect;
  let tooltipPos = $state<{ top: number; left: number }>({ top: 0, left: 0 });
  let hoveringEL: HTMLButtonElement | null = null;

  const availablePositions = (hoveringEL: HTMLElement): void => {
    const hoveringRect = hoveringEL.getBoundingClientRect();

    // viewport dimensions
    const vw = window.innerWidth;
    const vh = window.innerHeight;

    // check available space around the element
    const spaceTop = hoveringRect.top;
    const spaceBottom = vh - hoveringRect.bottom;
    const spaceLeft = hoveringRect.left;
    const spaceRight = vw - hoveringRect.right;

    // const top = hoveringRect.top - tooltipRect.height;
    // const left = hoveringRect.left - tooltipRect.width;
    // const right = vw - hoveringRect.right - tooltipRect.width;
    // const bottom = vh - hoveringRect.bottom - tooltipRect.height;
    const top = spaceTop >= tooltipRect?.height;
    const bottom = spaceBottom >= tooltipRect?.height;
    const left = spaceLeft >= tooltipRect?.width;
    const right = spaceRight >= tooltipRect?.width;
    console.log(top, left, right, bottom);
    positions = {
      top: hoveringRect.top - tooltipRect?.height,
      left: hoveringRect.left - tooltipRect?.width,
      right: vw - hoveringRect.right - tooltipRect?.width,
      bottom: vw - hoveringRect.right - tooltipRect?.width,
    };
  };
  const showTooltip = () => {
    const { top, left, right, bottom } = availablePositions(hoveringEL);
    console.log(top, left, right, bottom);
  };
  onMount(() => {
    console.log('hoveringEL', hoveringEL);
    const tooltipWrapper = document.querySelector(
      '.tooltip-wrapper',
    ) as HTMLDomElement;
    // const tooltipPanel = tooltipWrapper.chi;dren[0] as HTMLElement;
    if (tooltipWrapper) {
      tooltipRect =
        tooltipWrapper.children[0]?.getBoundingClientRect() as DOMRect;
    } else {
      console.log('tooltipSnippet not found');
    }
    showTooltip();
  });
</script>

<a href="/">Home</a>
<p style="color:red;margin-left:1rem;">
  availablePositions(...) is undefined and tru/false Positions list
</p>

{#snippet userDetails()}
  <div class="tooltip-panel">
    <p style="color:lightgreen;font-size:22px;margin:0;">
      Filip Isakovic, Junior
    </p>
    <p>6524 Cascade St.</p>
    <p>San Diego, 92122</p>
    <p>California</p>
  </div>
{/snippet}
<button onmouseenter={showTooltip} bind:this={hoveringEL}
  >hovering element
</button>
<div style="margin:60rem 80rem;">making view</div>
<div class="tooltip-wrapper">
  {@render userDetails()}
</div>

<style>
  button {
    position: absolute;
    top: 10rem;
    left: 15rem;
  }
  .tooltip-panel {
    position: absolute;
    top: 5rem;
    left: 10rem;
    width: max-content;
    padding: 0 1rem 0.5rem 1rem;
    border: 3px solid yellow;
    border-radius: 5px;
    color: yellow;
    background-color: navy;
    p {
      color: yellow;
      margin: 2px;
      padding: 0;
      text-align: center;
      height: 1rem;
      font-size: 14px;
    }
    p:first-child {
      color: skyblue;
      font-family: Times;
      font-size: 22px;
      padding: 3px 0 8px 0;
    }
  }
</style>
