<script lang="ts">
  import Tooltip from '$lib/components/Tooltip.svelte';

  // Example printReport function
  function printReport() {
    console.log('Printing report...');
  }
</script>

<a href="/">Home</a>

<!-- <div class="tooltip-panel">
  <p style="color:lightgreen;font-size:22px;margin:0;">Matia Isakovic</p>
  <p>6524 Cascade St.</p>
  <p>San Diego, 92122</p>
  <p>California</p>
</div> -->
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
<div style="height: 2000px; padding: 100px;position:relative;">
  <Tooltip
    preferredPositions={['top', 'left', 'right', 'bottom']}
    {tooltipPanel}
  >
    <div class="tooltip-wrapper">
      <p style="color:lightgreen;font-size:22px;margin:0;">
        Filip Isakovic, Junior
      </p>
      <p>6524 Cascade St.</p>
      <p>San Diego, 92122</p>
      <p>California</p>
      <button onclick={printReport}>Print Report</button>
    </div>
  </Tooltip>
</div>

<style lang="scss">
  .tooltip-panel {
    position: absolute;
    top: 6rem;
    left: 8rem;
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
    position: absolute;
    top: 5rem;
    left: 4rem;
    width: max-content;
    padding: 1re1.5rem;
    border: 3px solid yellow;
    border-radius: 5px;
    margin: 15rem 0 0 20rem;
    z-index: 10;
  }
</style>
