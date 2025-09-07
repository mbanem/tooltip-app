<script lang="ts">
  import { onMount } from 'svelte';
  import type { PositionAvailability } from './types'; // Define interface in types.ts

  let tooltipPanelEl: HTMLDivElement | undefined;
  let button: HTMLButtonElement | null = null;
  // let ttPosition = $state<boolean>(false);
  let tooltipPosition = $state<string | null>(null);

  // Function to get available positions
  function availablePositions(
    tooltipEl: HTMLDivElement | undefined,
  ): PositionAvailability {
    if (!button || !tooltipEl) {
      return { top: false, left: false, right: false, bottom: false };
    }

    // Temporarily make tooltip visible to measure dimensions (if hidden)
    const wasHidden = tooltipEl.style.display === 'none';
    if (wasHidden) {
      tooltipEl.style.display = 'block';
      tooltipEl.style.visibility = 'hidden'; // Keep invisible to user
      tooltipEl.style.position = 'absolute'; // Prevent layout shift
    }

    // Get tooltip dimensions
    const tooltipWidth = tooltipEl.offsetWidth;
    const tooltipHeight = tooltipEl.offsetHeight;

    // Restore original styles
    if (wasHidden) {
      tooltipEl.style.display = 'none';
      tooltipEl.style.visibility = '';
      tooltipEl.style.position = '';
    }

    const rect = button.getBoundingClientRect();
    const viewportWidth = window.innerWidth;
    const viewportHeight = window.innerHeight;

    // Check availability for each position
    const topAvailable = rect.top >= tooltipHeight; // Enough space above
    const leftAvailable = rect.left >= tooltipWidth; // Enough space to the left
    const rightAvailable = rect.right + tooltipWidth <= viewportWidth; // Enough space to the right
    const bottomAvailable = rect.bottom + tooltipHeight <= viewportHeight; // Enough space below

    return {
      top: topAvailable,
      left: leftAvailable,
      right: rightAvailable,
      bottom: bottomAvailable,
    };
  }

  // Pick a random available position
  function getRandomPosition(available: PositionAvailability): string | null {
    const validPositions = Object.entries(available)
      .filter(([_, isAvailable]) => isAvailable)
      .map(([position]) => position);
    if (validPositions.length === 0) return null;
    return validPositions[Math.floor(Math.random() * validPositions.length)];
  }

  // Handle hover to show tooltip
  function showTooltip() {
    // ttPosition = true;
    const positions = availablePositions(tooltipPanelEl);
    let { top, left, right, bottom } = positions;
    console.log('top', top, 'left', left, 'right', right, 'bottom', bottom);
  }

  // Hide tooltip
  function hideTooltip() {
    // ttPosition = false;
    tooltipPosition = null;
  }

  // Position tooltip based on selected position
  // $: tooltipStyle = tooltipPosition && tooltipPanelEl
  //   ? computeTooltipStyle(tooltipPosition, document.getElementById('x')?.getBoundingClientRect())
  //   : {};
  let tooltipStyle = $derived(
    tooltipPosition && tooltipPanelEl
      ? computeTooltipStyle(
          tooltipPosition,
          document.getElementById('x')?.getBoundingClientRect(),
        )
      : {},
  );
  function computeTooltipStyle(
    position: string,
    buttonRect: DOMRect | undefined,
  ) {
    if (!buttonRect || !tooltipPanelEl) return {};
    const offset = 10; // Space between button and tooltip
    const styles: { [key: string]: string } = {
      position: 'fixed',
      display: 'block',
    };

    switch (position) {
      case 'top':
        styles.top = `${buttonRect.top - tooltipPanelEl.offsetHeight - offset}px`;
        styles.left = `${buttonRect.left + buttonRect.width / 2 - tooltipPanelEl.offsetWidth / 2}px`;
        break;
      case 'bottom':
        styles.top = `${buttonRect.bottom + offset}px`;
        styles.left = `${buttonRect.left + buttonRect.width / 2 - tooltipPanelEl.offsetWidth / 2}px`;
        break;
      case 'left':
        styles.top = `${buttonRect.top + buttonRect.height / 2 - tooltipPanelEl.offsetHeight / 2}px`;
        styles.left = `${buttonRect.left - tooltipPanelEl.offsetWidth - offset}px`;
        break;
      case 'right':
        styles.top = `${buttonRect.top + buttonRect.height / 2 - tooltipPanelEl.offsetHeight / 2}px`;
        styles.left = `${buttonRect.right + offset}px`;
        break;
    }
    return styles;
  }
  // let ttStyle = {top:'6rem', top:'2rem'}
  onMount(() => {
    const button = document.getElementById('x');
  });
</script>

<a href="/">Home</a>

<div>
  tooltipPosition {tooltipPosition}
  <p style="color:blue;">shows as true/false list</p>
</div>
<button id="x" on:mouseenter={showTooltip} on:mouseleave={hideTooltip}
  >Hover Me</button
>
<div
  bind:this={tooltipPanelEl}
  style:display={tooltipPosition ? 'block' : 'none'}
  style:position="fixed"
  style:top={tooltipStyle.top}
  style:left={tooltipStyle.left}
  class="tooltip"
>
  Tooltip Content
</div>

<!-- <div style='margin:80rem 120rem;'>make scrolling view</div> -->
<style>
  .tooltip {
    background: #333;
    color: white;
    padding: 8px;
    border-radius: 4px;
    z-index: 1000;
    white-space: nowrap; /* Prevent text wrapping for consistent width */
  }
  button {
    position: absolute;
    top: 4rem;
    left: 5rem;
    width: max-content;
    padding: 3px 1rem;
  }
</style>
