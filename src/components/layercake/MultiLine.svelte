<!--
  @component
  Generates an SVG multi-series line chart. It expects your data to be an array of objects, each with a `values` key that is an array of data objects.
 -->
 <script>
    import { getContext } from 'svelte';
    const { data, xGet, yGet, zGet, xScale, yScale, xRange } = getContext('LayerCake');
  
    $: path = values => {
      return 'M' + values.filter(d => d.value !== 0 && d.value !=="" ) // filter skips drawing for points with undefined data values
        .map(d => {
          return $xGet(d) + ',' + $yGet(d);
        })
        .join('L');
    };
  </script>
  
  <g class="line-group">
    {#each $data as group, i}
      <path
        class='path-line line-{i}'
        d='{path(group.values)}'
        stroke="{$zGet(group)}"
      ></path>
      
      {#each group.values as v}
        {#if ($xGet(v) == $xRange[1]) && (v.value !== 0 && v.value !=='')}
          <circle cx={$xGet(v)} cy={$yGet(v)} r={5} fill="{$zGet(group)}"></circle>
        {/if}
      {/each}
    {/each}
  </g>

  <line class="divider-line" y1="{$yScale(-10)}" y2="{$yScale(800)}" x1="{$xScale("2019")}" x2="{$xScale("2019")}" style="stroke:#222"/>
  
  <style>
    .path-line {
      fill: none;
      stroke-linejoin: round;
      stroke-linecap: round;
      stroke-width: 3px;
    }
  </style>