## CRTooltip component

_CRTooltip could accept the following props, though all are optional_

```typescript
type TProps = {
  delay?: number; // transform params delay duration and baseScale
  duration?: number;
  baseScale?: number;
  caption?: string; // caption, a string, and tooltipPanel are mutually exclusive.
  // caption string can be styled by CSS style string sent as captionCSSStyle prop

  captionCSSStyle?: string; // user styling as a CSS string that overrides CRTooltip provided default-caption
  // CSS string; e.g. captionCSSStyle='font-size:14px; color:orange;'
  tooltipPanel?: TPanel; // snippet object defined by user and it is sent by object name to component via $props()
  // if caption and tooltipPanel snippet name are both specified caption is ignored
  // e.g. for {#snippet userDetails(user)} we specify $props()
  // tooltipPanel={userDetails}   -- a function reference, no a  name as string
  // NOTE: when neither tooltipPanel nor caption are specified CRTooltip throws an error 'tooltipPanel or caption is mandatory'

  tooltipPanelCSSClassName?: string; // user could send tooltipPanel's CSS class to CRTooltip to apply, but the CSS
  // class name e.g. for userDetails snippet is named :global(.css-prop-class-userDetails)
  // ans in CSS it must be encapsulated with :global() in order to propagate tp the component
  // :global(.css-prop-class-userDetails){ CSS Roles... }

  children?: Snippet; // Any HTML markup content between <Tooltip> children... </Tooltip> tags.
  // Children is a hovering element triggering tooltip visibility via mouseenter/mouseleave
  // so children HTML markup is usually encapsulated in a single HTML element

  preferredPos?: string; // When, due to scrolling, there is a lack of space around the hovering element CRTooltip
  // tries to find available space following the recommended sequence by user from the
  // preferredPos prop string or, if not specified, use the default one 'top,left,right,bottom'
};
```

#### An example of a Svelte 5 page using CRTooltip component

```typescript
<script lang='ts'>
  import Tooltip from $lib/components/CRTooltip.svelte

  const props = {
    delay: 250,
    duration: 800,
    baseScale: 0,
    preferredPos: 'top,right,left,bottom',
  };

  const printReport = () => {
    console.log('Printing the Report...)
  }
</script>
  <!-- snippet function with optional style argument -->
  {#snippet userDetails(styleCSS?: string)}
    <div class="tooltip-panel" style={styleCSS ? styleCSS : ''}>
      <p style="color:lightgreen;font-size:22px;margin:0;">
        John Doe
      </p>
      <p>1100 Cascade St.</p>
      <p>San Diego, 92122</p>
      <p>California</p>
    </div>
  {/snippet}
```

````html
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
  the specified or default preferredPos
  </pre>

  <Tooltip
    {...props}
    preferredPos={preferPos}
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

  /* the tooltipPanel snippet renders HTML markup
     so several different parts are styled as follows
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
     CRTooltip expect only one HTML element as output
     so for a complex markup we need a wrapper element
  */
  .tooltip-wrapper {
    width: max-content;
    margin: 55rem 0 0 80rem;
    pre {
      margin-left: 12rem;
    }
  }
  /* when sent as a prop no leading dot should be specified
     as the Tooltip is an external to this content it has to
     be treated as :global()
  */
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

  /* when sent as a prop no leading dot should be specified */
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

  /* hovering element is the first child or a
     wrapper HTML element for a more complex markup
  */
  .hovering-element {
    margin: 0; //8rem 0 0 18rem;
    padding: 1rem;
  }
</style>
````
