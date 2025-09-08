<script lang="ts">
  import Tooltip from '$lib/components/CRTooltip.svelte';

  function printReport() {
    alert('Report printed!');
  }
  let preferredPositions = 'top,left,right,bottom';
</script>

<a href="/">Home</a>

{#snippet tooltipPanel()}
  <div class="tooltip-panel">
    <p style="color:lightgreen;font-size:22px;margin:0;">
      Filip Isakovic, Junior
    </p>
    <p>6524 Cascade St.</p>
    <p>San Diego, 92122</p>
    <p>California</p>
  </div>
{/snippet}

<Tooltip {tooltipPanel} preferredPosition={preferredPositions}>
  <button id="x" onclick={printReport}>Print Report</button>
</Tooltip>
<p style="color:red;margin-left:1rem;">top position clips the tooltip</p>

<style lang="scss">
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
</style>
