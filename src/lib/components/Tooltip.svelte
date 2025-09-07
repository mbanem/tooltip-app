<script lang="ts">
  import { cubicInOut } from 'svelte/easing'; // for animated transition
  import { type Snippet, onMount } from 'svelte';

  import type { EasingFunction } from 'svelte/transition';

  // fade scale animation for displaying/hiding tooltip
  export interface FadeScaleParams {
    delay?: number;
    duration?: number;
    easing?: EasingFunction;
    baseScale?: number;
    translateX?: string;
    translateY?: string;
  }

  const fadeScale = <IProps extends FadeScaleParams>(
    node: HTMLElement,
    {
      delay = 100,
      duration = 1600,
      easing = (x: number) => x,
      baseScale = 0,
      translateX = '1rem',
      translateY = '-160%',
    }: IProps,
  ) => {
    const opacity = +getComputedStyle(node).opacity;
    const m = getComputedStyle(node).transform.match(/scale\(([0-9.]+)\)/);
    const scale = m ? Number(m[1]) : 1;
    const is = 1 - baseScale;
    // console.log(translateX, translateY)
    // transform: translate uses matrix's last two entries for translate x and y
    // with scaleX=1 skewX=0 skewY=0  scaleY=1 (1-no scale and 0-no skew) just translate
    // NOTE: transform: translate is defined in the Tooltip.svelte and must specify
    // the same left/top values as the one in this css return value
    return {
      delay,
      duration,
      css: (t: number) => {
        const eased = easing(t);
        return `opacity: ${eased * opacity}; transform: translate(${translateX},${translateY}) scale(${eased * scale * is + baseScale}) `;
      },
    };
  };

  const sixHash = () => {
    const a = (Math.random() * 46656) | 0;
    const b = (Math.random() * 46656) | 0;
    return a.toString(36).slice(-3) + b.toString(36).slice(-3);
  };

  const hoveringId = 'hovering-' + sixHash();
  // as caption and tooltipPanel are mutually exclusive
  // even when both are received via $props()
  // we use the same tooltipPanelId for both
  // const tooltipPanelId = 'tooltip-' + sixHash();
  let tooltipPanelEl = $state<HTMLElement | null>(null);
  const round = Math.round;

  type TPanelArgs = any[]; //[string, string, boolean];
  type TPanel = ((className?: string) => ReturnType<Snippet>) | null;

  type TProps = {
    delay?: number;
    duration?: number;
    baseScale?: number;
    caption?: string;
    captionCSS?: string;
    tooltipPanel?: Snippet<[...any[]]> | null;
    panelArgs?: TPanelArgs; // arguments to forward
    children?: Snippet;
    preferredPos?: string;
    toolbarHeight?: number;
    top?: string;
    left?: string;
  };

  let {
    duration = 1000,
    delay = 800,
    baseScale = 0,
    caption = '',
    captionCSS = '',
    tooltipPanel,
    panelArgs, // arguments to forward
    children,
    preferredPos = 'top,left,right,bottom',
    toolbarHeight = 0,
    top = '0',
    left = '0',
  }: TProps = $props();

  console.log('tooltipPanel', tooltipPanel);
  console.log('panelArgs', panelArgs);
  // console.log('captionCSS', captionCSS);
  // Need to define variables as the setTooltipPos function adjusted them
  // to position properly based on preferredPos settings and available
  // space around the hovering elements
  let translateX = $state<string>('');
  let translateY = $state<string>('');
</script>

<!-- 
    NOTE: transform:translate is defined in the fade-scale and must specify
    the same left/top values as the one in this tooltipPanelEl handler
-->
<div
  style="position:absolute;top:{top};left:{left};"
  bind:this={tooltipPanelEl}
  class="ttWrapper"
>
  Here we render tooltip with arguments
  {@render children?.()}
  {@render tooltipPanel?.(panelArgs)}
</div>

<style>
  .child-wrapper {
    margin: 3rem 0 0 16rem; /* global position */
    padding: 0;
    width: max-content;
    height: auto;
    border: none;
    outline: none;
    z-index: 10;
  }
  .ttWrapper {
    width: max-content;
    /*height: auto;*/
    margin: 1rem !important;
    padding: 6px 1rem !important;
    border: none;
    outline: none;
  }
  .caption-default {
    border: 1px solid yellow;
    border-radius: 5px;
    color: yellow;
    background-color: navy;
    width: max-content;
    padding: 3px 1rem;
    margin: 0 !important;
    text-align: center;
    font-size: 14px;
    font-family: Arial, Helvetica, sans-serif;
    z-index: 10;
  }
</style>
