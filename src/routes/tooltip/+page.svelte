<script lang="ts">
  import Tooltip from '$lib/components/CRTooltip.svelte';
  import { onMount } from 'svelte';
  import { afterNavigate } from '$app/navigation';

  export function scrollToPosition(top: number, left: number) {
    afterNavigate(() => {
      window.scroll({ top, left, behavior: 'smooth' });
    });
  }

  const props = {
    delay: 250,
    duration: 800,
    baseScale: 0,
    // preferredPos: preferPos,
  };
  const printReport = () => {
    console.log('printing the report...');
  };

  let preferPos = $state<string>('top,left,right,bottom,');
  // Five buttons to set preferred position or reset the list
  const setPosList = (e: MouseEvent) => {
    const target = e.target as HTMLButtonElement;
    const pos = target.textContent;
    if (preferPos.includes(pos)) {
      // to relocate first remove the position from the list
      // and then prepend it to the list
      preferPos = preferPos.replace(pos + ',', '');
      preferPos = pos + ',' + preferPos;
    } else {
      // set the default list
      if (pos === 'reset') {
        preferPos = 'top,left,right,bottom';
      } else {
        preferPos += pos + ',';
      }
    }
  };
  let captionContainer: HTMLDivElement | null = null;
  onMount(() => {
    // window.scroll({
    //   top: 450,
    //   left: 800,
    //   behavior: 'smooth',
    // });

    // to enable testing tooltip repositioning on scroll
    scrollToPosition(55 * 16, 80 * 16);
  });
</script>

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

<div class="tooltip-wrapper">
  <Tooltip
    {...props}
    tooltipPanel={userDetails}
    caption={'Click to Print Tooltip Panel -- ignored as userDetails is provided'}
    preferredPos={preferPos}
  >
    <button class="hovering-button" onclick={printReport}>
      Filip Isakovic
    </button>
  </Tooltip>
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
    preferredPos={preferPos}
    caption="caption: This is the caption"
    captionCSS="css-prop-class-caption"
  >
    <button class="hovering-button" onclick={printReport}>
      child: Tooltip with caption only
    </button>
  </Tooltip>
</div>

<!-- TEST if first preferred has no space try succeeding one by one -->
<!-- <p style="margin:100rem 0 0 130rem">100rem x 100rem</p> -->
<div
  style="position:absolute;top:140rem;left:280rem;width:14rem;margin-bottom:2rem !important;"
>
  enabling page scrolling
</div>
<div style="position:absolute;top:94rem;left:114rem;">shim</div>

<style lang="scss">
  @mixin gradient-text(
    $font: italic bold 50px/50px 'Helvetica',
    $color-from: red,
    $color-to: royalblue
  ) {
    font: $font;
    // font: italic small-caps bold 60px/60px Georgia, "Comic Sans MS", serif;
    background: linear-gradient(to right, $color-from, yellow, $color-to);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    width: max-content;
    height: auto;
  }

  /* class sent as prop to component must be wrapped in :global() */

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
  :global(.css-prop-class-userDetails) {
    position: absolute;
    top: 0 !important;
    left: 0;
    color: navy;
    background-color: skyblue;
    width: 14.5rem;
    padding: 3px 1.4rem;
    border-radius: 5px;
    margin: 2rem 0 0 2.3rem;
    text-align: center;
    z-index: 10;
  }

  .grid-wrapper {
    display: grid;
    grid-template-columns: 1fr 2fr;
    gap: 1rem;
    padding: 0;
    .summary-mostly-parent {
      font-size: 1.2rem;
      color: var(--PRE-COLOR);
      margin-inline-start: 1rem;
      /* should be instead of margin-left in above details > p */
      list-style-position: outside;
      margin-left: 3rem;
      cursor: pointer;
      width: max-content;
      padding: 0 0.5rem;
    }
  }
  :global(.css-prop-class-caption) {
    font-size: 14px;
    color: skyblue;
    width: max-content;
    height: 1rem !important;
    padding: 0 1rem !important;
    border-radius: 5px;
    border: 1px solid skyblue;
    background-color: navy;
    text-align: center;
    margin: 0;
    outline: none;
  }
  .text-gradient {
    @include gradient-text();
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
  /* ------------  scroller ------------ */
  .scroller {
    margin: 0;
  }
  .container {
    margin-left: 3rem;
    padding: 0;
  }
  .viewport {
    overflow: hidden;
    width: 100px;
    /* border: 1px solid gray; */
    border-radius: 5px;
    color: navy;
    background-color: cornsilk;
    margin-left: 1.6rem;
    margin-top: 1px;
  }

  .row {
    display: flex;
    width: 300%; /* 3 items */
    margin-top: 0; /* to suppress interfering with animation*/
    margin: 0;
    margin-left: -100%;
  }

  .number-box {
    width: 100px;
    flex-shrink: 0;
    text-align: center;
    font-size: 2em;
  }

  .buttons {
    margin-top: 1rem;
  }
  .radio-wrapper {
    // position: absolute;
    // top: 31rem;
    // left: 12rem;
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
