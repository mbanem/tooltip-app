<script lang="ts">
  import { onMount, type Snippet } from 'svelte';
  import Tooltip from '$lib/components/CRTooltip.svelte'; // Adjust the path as necessary
  onMount(() => {});

  type TPanelArgs = any[]; //[string, string, boolean];
</script>

<a href="/">Home</a>

{#snippet userDetails([
  fName,
  lName,
  address,
  city,
  state,
  isAdmin,
]: TPanelArgs)}
  <div class="tooltip-panel">
    <p style="color:lightgreen;font-size:22px;margin:0;">
      {fName}
      {lName}
      {isAdmin ? '(admin)' : '(user)'}
    </p>
    <p>{address}</p>
    <p>{city}</p>
    <p>{state}</p>
  </div>
{/snippet}

<div class="tooltip-wrapper">
  <div style="position:absolute:top:0;left:0">
    <Tooltip
      tooltipPanel={userDetails}
      panelArgs={[
        'Bane:',
        'Milutinovic',
        '118 Princess St',
        'Toronto M5A 0B1',
        'Ontario',
        true,
      ]}
      preferredPos="top,left,bottom,right"
    >
      <button>Bane Milutinovic top</button>
    </Tooltip>
    <Tooltip
      tooltipPanel={userDetails}
      panelArgs={[
        'Filip:',
        'Isakovic',
        '6425 Cascade St.',
        'San Diego 92122',
        'California',
        true,
      ]}
      preferredPos="left,top,bottom,right"
    >
      <button>John Doe (admin) left</button>
    </Tooltip>
  </div>
  <div style="position:absolute;top:0;left:25rem;">
    <Tooltip
      tooltipPanel={userDetails}
      panelArgs={[
        'Matia:',
        'Isakovic',
        '2210 Main St.',
        'New York12332;',
        'New York',
        false,
      ]}
      preferredPos="right,left,top,right,left,bottom"
    >
      <button>Moderator right</button>
    </Tooltip>
    <Tooltip
      tooltipPanel={userDetails}
      panelArgs={[
        'Ljuban:',
        'Milutinovic',
        '100 Coe Hill Dr.',
        'Toronto M4A 0B1;',
        'Ontario',
        false,
      ]}
      preferredPos="bottom,top,right,left"
    >
      <button>Moderator right</button>
    </Tooltip>
  </div>
</div>
<Tooltip
  caption="Matia, Isakovic, 2210 Main St. New York12332; New York (user)"
  captionCSS="color:white;"
  preferredPos="top,right,left,bottom"
>
  <button>Moderator top</button>
</Tooltip>
<Tooltip
  caption="Marko, Milutinovic, 100 Coe Hill Dr. Toronto M4A 0B1; Ontario (user)"
  captionCSS="cap"
  preferredPos="right,top,left,bottom"
>
  <button>Marko right</button>
</Tooltip>
<Tooltip
  caption="Mia, Milutinovic, 100 Coe Hill Dr. Toronto M4A 0B1; Ontario (user)"
  captionCSS="caption-css"
  preferredPos="top,right,left,bottom"
>
  <button>Mia top</button>
</Tooltip>
<div style="margin-top:20rem;margin-left:30rem;"></div>

<style>
  button {
    /* position: absolute;
    top: 10rem;
    left: 15rem; */
    padding: 0.5rem 1rem;
    font-size: 16px;
    cursor: pointer;
    border: 2px solid yellow !important;
  }
  .tooltip-panel {
    /* position: absolute;
    top: 5rem;
    left: 0; */
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
    /* position: relative; */
    margin-top: 10rem;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
  }
  :global(.cap) {
    background-color: green !important;
  }
  :global(.caption-css) {
    font-size: 18px;
    width: max-content;
    text-wrap: nowrap;
    border: 1px solid yellow;
    padding: 7px 1rem;
    color: yellow;
    background-color: navy;
    border-radius: 6px;
  }
</style>
