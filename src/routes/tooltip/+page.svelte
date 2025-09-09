<script lang="ts">
  import Tooltip from '$lib/components/CRTooltip.svelte';
  import { onMount } from 'svelte';
  import { afterNavigate } from '$app/navigation';

  const scrollToPosition = (top: number, left: number) => {
    afterNavigate(() => {
      window.scroll({ top, left, behavior: 'smooth' });
    });
  };

  // caption styling could be done via CSS class name or a style string
  // NOTE: If page has a toolbar from a layout its height would impact calculation of the proper
  //       tooltip position required by preferredPosition, so its height as a number of pixels should
  //       be sent via props, e.g. toolbarHeight: 32  not '32px'

  const props = {
    delay: 250,
    duration: 800,
    baseScale: 0,
    toolbarHeight: 32,
    captionCSS: `
    border: 1px solid yellow;
    border-radius: 5px;
    color: yellow;
    background-color: navy;
    width: max-content;
    padding: 3px 1rem;
    margin: 0;
    text-align: center;
    font-size: 14px;
    font-family: Arial, Helvetica, sans-serif;
    z-index: 10;
    `,
    // preferredPosition: preferPos,   // specify among props or inside <Tooltip> see below
  };
  const printReport = () => {
    console.log('printing the report...');
  };

  let preferPos = $state<string>('top,left,right,bottom,');

  // For testing there are five buttons to set preferred position list or reset it
  const setPosList = (e: MouseEvent) => {
    const target = e.target as HTMLButtonElement;
    const pos = target.textContent;

    if (preferPos.includes(pos)) {
      // to relocate a position first remove it from the CSV list
      // and then prepend it as the first element to the list
      preferPos = preferPos.replace(pos + ',', '');
      preferPos = pos + ',' + preferPos;
    } else {
      // set the default position CSV list
      if (pos === 'reset') {
        preferPos = 'top,left,right,bottom';
      } else {
        preferPos += pos + ',';
      }
    }
  };

  onMount(() => {
    // to enable testing tooltip repositioning with view scrolling
    // to make inadequate space for some position we force the
    // tooltip to find an available position from the current CSV list
    scrollToPosition(55 * 16, 80 * 16);
  });
</script>

<!-- render tooltipPanel snippet with complete CSS styling 
  For tooltipPanel it is inconvenient to send CSS class to
  the component in order to send it back as userDetail snippet
  arg so the local CSS class should be applied.
  For captions on the other hand the accompanying CSS class 
  should be sent as a prop if the caption needs some styling
-->
<!-- a tooltip panel with a local class -->
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

<!-- 
  For testing purposes the tooltip-wrapper position the markup block with hovering 
  elements in the middle of an expanded view so the scrolling could put hovering
  elements in a position inadequate for the leading positions in a CSV position
  string 'top,left,bottom,right' ans so force the Tooltip to find a proper one
  iterating over the current CSV position string
-->
<div class="tooltip-wrapper">
  <Tooltip
    {...props}
    panel={userDetails}
    caption={'Click to Print Tooltip Panel -- ignored as userDetails is provided'}
    preferredPosition={preferPos}
  >
    <button class="hovering-button" onclick={printReport}>
      Filip Isakovic
    </button>
  </Tooltip>
  <p style="color:blue;">Positions are OK</p>
  <p style="margin-left:12rem;">Change Preferred Tooltip Position</p>
  <input class="input" bind:value={preferPos} />
  <p class="preferable-info">Clicking a button sets it preferable</p>
  <div class="radio-wrapper">
    {#each ['top', 'left', 'right', 'bottom', 'reset'] as title}
      <button onclick={setPosList}>{title}</button>
    {/each}
  </div>
  <pre class="scroll-info">
      Hover over button to  trigger a tooltip.
      
      Scroll to make no space available for
      top or left so tooltip should examine
      next available positions from the input
      box that you can enter the next list
    </pre>
  <Tooltip
    {...props}
    preferredPosition={preferPos}
    caption="caption: This is the caption"
  >
    <button class="hovering-button" onclick={printReport}>
      child: Tooltip with caption only
    </button>
  </Tooltip>
</div>

<!-- TEST if first preferred position has no space then try the succeeding one by one -->
<div
  style="position:absolute;top:140rem;left:280rem;width:14rem;margin-bottom:2rem !important;"
>
  enabling page scrolling
</div>
<div style="position:absolute;top:94rem;left:114rem;">shim</div>

<style lang="scss">
  /* class sent as prop to component must be wrapped in :global() */
  :global(.css-prop-class-caption) {
    border: 1px solid yellow;
    border-radius: 5px;
    color: yellow;
    background-color: navy;
    width: max-content;
    padding: 3px 1rem;
    margin: 0;
    text-align: center;
    font-size: 14px;
    font-family: Arial, Helvetica, sans-serif;
    z-index: 10;
  }

  .tooltip-panel {
    position: absolute;
    top: 0;
    left: 0;
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
  .tooltip-wrapper {
    width: max-content;
    margin: 55rem 0 0 80rem;
    pre,
    .input {
      margin-left: 12rem;
    }
  }
  .hovering-button {
    margin: 0; //8rem 0 0 18rem;
    padding: 1rem;
  }
  .input {
    color: navy;
    padding: 2px 0.5rem;
    text-align: center;
    font-size: 17px;
    color: navy;
  }
  .radio-wrapper {
    margin-left: 11rem;
    display: flex;
    gap: 1rem;
    margin-bottom: 2rem;
    label {
      margin-top: 2rem;
    }
  }
  .preferable-info {
    margin-left: 12rem;
    color: gray;
    font-size: 0.9rem;
  }
  .scroll-info {
    margin-left: 10rem !important;
  }
</style>
