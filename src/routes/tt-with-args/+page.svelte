<script lang="ts">
  import Tooltip from '$lib/components/CRTooltip.svelte'; // Adjust the path as necessary

  type TPanelArgs = Array<number | string | boolean>;
</script>

{#snippet userDetails([
  fName,
  lName,
  address,
  city,
  zip,
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
    <p>{city} {zip}</p>
    <p>{state}</p>
  </div>
{/snippet}

<div class="tooltip-wrapper">
  <div>
    <Tooltip
      panel={userDetails}
      panelArgs={[
        'Cindy',
        'Williams',
        '1518 Langley Ave',
        'Naples',
        34234,
        'Florida',
        true,
      ]}
      preferredPosition="top,left,bottom,right"
    >
      <button>Cindy Williams top</button>
    </Tooltip>
    <Tooltip
      panel={userDetails}
      panelArgs={[
        'John',
        'Doe',
        '122 Cascade St.',
        'San Diego',
        92122,
        'California',
        true,
      ]}
      preferredPosition="right,left,top,bottom"
    >
      <button>John Doe (admin) right</button>
    </Tooltip>
  </div>
  <div>
    <Tooltip
      panel={userDetails}
      panelArgs={[
        'Eleanore',
        'Wilson',
        '2210 Main St.',
        'New York;',
        11202,
        'New York',
        false,
      ]}
      preferredPosition="right,left,top,bottom"
    >
      <button>Moderator right</button>
    </Tooltip>
    <Tooltip
      panel={userDetails}
      panelArgs={[
        'Amanda',
        'Holden',
        '100 East Avenue',
        'London M4A 0B1;',
        4431,
        'United Kingdom',
        false,
      ]}
      preferredPosition="bottom,top,right,left"
    >
      <button>Amanda Holden bottom</button>
    </Tooltip>
    <pre>
The page uses the same snippet panel that accepts tuple arguments defined via 
  type <snap style="color:red;">TPanelArgs</snap
      > = Array&lt;number | string | boolean&gt;;
When defining the type of array elements we actually define tuple
(an array of fixed length with every element of a proper type)
and the snippet is defined as
  &lcub;#snippet userDetails([fName,lName,address,city,state,isAdmin,zip]: <snap
        style="color:red;">TPanelArgs</snap
      >)&rcub;
    &lt;div class="tooltip-panel"&gt;
      &lt;p style="color:lightgreen;font-size:22px;margin:0;"&gt;
        &lcub;fName&rcub;
        &lcub;lName&rcub;
        &lcub;isAdmin ? '(admin)' : '(user)'&rcub;
      &lt;/p&gt;
      &lt;p&gt;&lcub;address&rcub;&lt;/p&gt;
      &lt;p&gt;&lcub;city&rcub; &lcub;zip&rcub;&lt;/p&gt;
      &lt;p&gt;&lcub;state&rcub;&lt;/p&gt;
    &lt;/div&gt;
  &lcub;/snippet&rcub;
Tooltip component wraps children HTMLElement, here a button,
that the component turns into a hoverable element triggering
show/hide via onmouseenter/onmouseleave sending the following props
  &lt;div class="moderator-left"&gt;
    &lt;Tooltip
          panel=&lcub;userDetails&rcub;
          panelArgs=&lcub;[
            'Michael','Greenwood','2210 Main St.',12332,'New York;','New York',false,
          ]&rcub;
          preferredPosition="left,top,right,left,bottom"
    &gt;
      &lt;button&gt;Moderator left&lt;/button&gt;
    &lt/Tooltip&gt;
  &lt;/div&gt;

When using caption, a string or HTML String there are no arguments just
captionCSS -- a :global(class name) or style string and here there is no zip
    </pre>
  </div>
</div>
<Tooltip
  caption="Michael Greenwood, 2210 Main St. New York, New York (user)"
  captionCSS="color:white;border:8px solid skyblue;"
  preferredPosition="top,right,left,bottom"
>
  <button>caption top</button>
</Tooltip>
<Tooltip
  caption="Eleanore Wilson, 25 Coe Hill Dr. Toronto M4A 0B1; Ontario (user)"
  captionCSS="cap"
  preferredPosition="right,top,left,bottom"
>
  <button>caption right</button>
</Tooltip>
<pre style="margin:0;padding:0;">
              Hover above to see that we sent captionCSS prop to override component's  
              captionDefault background color navy to a new  background color green
</pre>
<Tooltip
  caption="Mia, McCartney, 100 Green Blvd, Atlanta 43112 Georgia (user)"
  captionCSS="caption-css"
  preferredPosition="top,right,left,bottom"
>
  <button>caption top</button>
</Tooltip>
<div style="margin-top:20rem;margin-left:30rem;"></div>

<style>
  button {
    padding: 0.5rem 1rem;
    font-size: 16px;
    cursor: pointer;
    border: 2px solid yellow !important;
  }
  .tooltip-panel {
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
      text-wrap: nowrap;
    }
  }
  .tooltip-wrapper {
    margin-top: 10rem;
    display: grid;
    grid-template-columns: 20rem 1fr;
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
  pre {
    margin-left: 10rem;
    color: navy;
  }
</style>
