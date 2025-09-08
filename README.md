## CRTooltip component

_CRTooltip could accept the following props, though all are optional_

```typescript
type TProps = {
  delay?: number; // transform params delay duration and baseScale
  duration?: number;
  baseScale?: number;

  caption?: string; // caption, a string, and tooltipPanel snippet are mutually exclusive.
  // The caption string can be styled by CSS style string or a class name
  // sent as captionCSS prop. When both tooltipPanel and caption are specified
  // inside the props the caption string is ignored

  captionCSS?: string; // user styling as a CSS class name or a style string applied
  // to style the caption string e.g. captionCSS="caption-class' with
  // :global(.caption-class){...} or with style captionCSS='font-size:14px; color:orange;'

  // When parent page has several hovering elements with tooltip captions the one class name
  // or a style string variable can be used in props structure with a name specific to that
  // hovering group e.g. <Tooltip captionCSS={captionCSSButtons}> and for another group
  // a different props structure could be defined for them <Tooltip captionCSS={captionCSSNotes}>
  // where let captionCSSButtons = 'font-size:14px;border:...';

  tooltipPanel?: TPanel; // A snippet object defined by parent page and sent as object name
  // to a component via $props(). If caption and tooltipPanel snippet names are both specified
  // the caption is ignored e.g. for {#snippet userDetails(user)} we specify $props()
  // tooltipPanel={userDetails}   -- a function reference, not as a string tooltipPanel="userDetails"

  children?: Snippet; // Any HTML markup content between <Tooltip> children... </Tooltip> tags.
  // Children are hovering elements triggering tooltip visibility via mouseenter/mouseleave so
  // children markup is usually encapsulated in a single HTML hovering element, which is recommended.

  preferredPosition?: string; // When, due to the scrolling, there is a lack of space around the hovering
  // element then CRTooltip tries to find an available space following the recommended sequence by the
  // preferredPosition prop string or, if not specified, by the default one 'top,left,right,bottom'.

  toolbarHeight?: string; // If a page has a toolbar from a layout its height would impact the calculation
  // of the proper tooltip position required by preferredPosition, so its toolbarHeight as a number should be
  // sent via props
};
```

#### An example of a Svelte 5 page using CRTooltip component

```typescript
<script lang='ts'>
  import Tooltip from $lib/components/CRTooltip.svelte

  // a single prop structure in this example while there could be several
  const props = {
    delay: 250,
    duration: 800,
    baseScale: 0,
    preferredPosition: 'top,right,left,bottom',
  };

  // a fake action triggered by a button
  const printReport = () => {
    console.log('Printing the Report...)
  }
</script>
  <!-- snippet function is recommended to be styled by a local
  CSS class and styles. Sending a CSS class via props is not
  always successful as the class should be defined global e.g.
     <style lang='scss'>
       :global(.user-details){...}
    </style>
    but the page and component CSS scopes differ and often only
    CSS rules that cascade by default are delivered rendering
    the tooltipPane inadequately.
   -->
 {#snippet userDetails()}
  <div class="tooltip-panel">
    <p style="color:lightgreen;font-size:22px;margin:0;">
      John Doe
    </p>
    <p>1212 Cascade St.</p>
    <p>San Diego, 92122</p>
    <p>California</p>
  </div>
{/snippet}
```

````html
<!-- in order to test situations where there not adequate space
 around the hoveringElement a class as tooltip-wrapper here
 could position <Tooltips> in the middle od a larger view so
 the hovering elements could be scrolled in test position
 -->
<div class="tooltip-wrapper">
  <Tooltip
    {...props}
    tooltipPanel={userDetails}
    caption={'Click to Print Tooltip Panel -- ignored as userDetails is provided'}
  >
    <button class="hovering-element" onclick={printReport}>
      Print Report
    </button>
  </Tooltip>

  <pre style="color:navy">
  Hover over button to  trigger a tooltip.

  Make large view to be able to scroll around
  and make no space available around hovering
  element for top or left so CRTooltip should
  examine the next available positions from
  the specified or default preferredPosition
  </pre>

  <Tooltip
    {...props}
    preferredPosition={preferPos}
    caption="caption: This is the tooltip as a caption"
    captionCSSStyle="css-prop-class-caption"

  >
    <!-- first child begin -->
    <button class="hovering-element" onclick={printReport}>
      child: Tooltip with caption only
    </button>
    <!-- first-child end -->
  </Tooltip>
</div>

<!-- TEST if first preferred has no space try succeeding one by one -->
<!-- <p style="margin:100rem 0 0 130rem">100rem x 100rem</p> -->
<div
  style="position:absolute;top:140rem;left:280rem;width:14rem;margin-bottom:2rem !important;"
>
</div>
<!--
 To make page scrollable uncomment this to make View bigger
 <div style="position:absolute;top:94rem;left:114rem;">shim</div> -->

```css
<style lang="scss">
  /* class sent as prop to component must be wrapped in :global() */

  /* when sent as a prop no leading dot should be specified
      as the Tooltip is an external to this content it has to
      be declared as :global()
  */

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
  /* the tooltipPanel snippet renders HTML markup
     so several different parts are styled as follows
     and that often makes problem for sending such a
     CSS class as global via props
  */
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

  /* In order to obtain the proper size of tooltipPanel
     necessary for proper positioning of the accompanying
     tooltip CRTooltip component expect only one HTML
     element as a wrapper for a complex markup.
  */
  .tooltip-wrapper {
    width: max-content;
    margin: 55rem 0 0 80rem;
    pre {
      margin-left: 12rem;
    }
  }


  /*
    hovering element is selected by CRTooltip as the first child
    so for a complex markup it should be an HTML wrapper element
  */
  .hovering-element {
    margin: 0; //8rem 0 0 18rem;
    padding: 1rem;
  }
</style>
````
