<script>
  import * as Sankey from 'd3-sankey';
  import data from "/src/data/data.js";

  const margin = {
    top: 20,
    right: 20,
    bottom: 20,
    left: 0,
  };

  $: width = 400;
  let height = 400;
  $: innerWidth = width - margin.left - margin.right;
  $: innerHeight = height - margin.top - margin.bottom;

  let nodes, links;
  $: {
    const sankey = Sankey()
      .nodeAlign(d3.sankeyJustify)
      .nodeWidth(5)
      .nodePadding(10)
      .nodeId(d => d.name)
      .size([$innerWidth, $innerHeight])
      .nodeSort(null)

  const dataSankey = sankey(data);
  nodes = dataSankey.nodes;
  links = dataSankey.links;
  }

</script>

<div class="chart-container" bind:clientWidth={width}>
  <svg {width} {height}>
    <g transform="translate({margin.left}, {margin.top})">
      {#each links as d}
        <path
        d={Sankey.sankeyLinkHorizontal(d)}
        fill='none'
        stroke='rgba(0, 0, 0, .2)'
        stroke-opacity='0.5'
        stroke-width={d.width} />
      {/each}
    </g>
    <g class='rect-group'>
      {#each nodes as d}
        <rect
          x={d.x0}
          y={d.y0}
          height={d.y1 - d.y0}
          width={d.x1 - d.x0}
          fill='#333' />
        <text
          x={d.x0 < $width / 4 ? d.x1 + 6 : d.x0 - 6}
          y={(d.y1 + d.y0) / 2}>
          {d.name}
        </text>
      {/each}
    </g>
  </svg>
</div>