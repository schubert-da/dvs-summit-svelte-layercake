<script>
	import { LayerCake, Svg, Html, groupLonger, flatten } from "layercake";

	import { scaleOrdinal } from "d3-scale";
  import { timeParse, timeFormat } from 'd3-time-format';

	import MultiLine from "$components/layercake/MultiLine.svelte";
	import AxisX from "$components/layercake/AxisX.svelte";
	import AxisY from "$components/layercake/AxisY.svelte";
	import Annotations from "$components/layercake/Annotations.html.svelte";
	import data from "$data/poverty-projection-data.csv";

	const xKey = "Year";
	const yKey = "value";
	const zKey = "type";

	const seriesNames = Object.keys(data[0]).filter((d) => d !== xKey);

	const groupedData = groupLonger(data, seriesNames, {
		groupTo: zKey,
		valueTo: yKey
	});

  // const xKeyCast = timeParse('%Y');
  // const formatTickX = timeFormat('%Y');

  data.forEach(d => {
    // d[xKey] = typeof d[xKey] === 'string'
    //   ? xKeyCast(d[xKey])
    //   : d[xKey];

    seriesNames.forEach(name => {
      d[name] = +d[name];
    });
  });

	const tempSize = 520;
	const tempTicks = Array.from({ length: 7 }, (_, i) =>
		Math.round(0 + (tempSize * i) / 6)
	);
  
	// const xScale = scaleOrdinal()
	// 	.domain(data.map((d) => d["Year"]))
	// 	.range(tempTicks);

  const annotations = [
		{
			text: "milllions of poor",
			top: "-40px",
			left: "-170px",
			class: "poverty-annot annot",
		},
		{
			text: "onset of<br>COVID-19 pandemic",
			bottom: "10px",
			right: "35%",
			class: "covid-onset-annot annot"
		},
		{
			text: "COVID-19 baseline <br> projection <br> <span style='font-weight:400'>731</span>",
			top: "-40px",
			right: "-10px",
			class: "baseline-proj-annot annot"
		},
		{
			text: "<span style='font-weight:400'>588</span> <br> Forecast before <br> COVID-19 ",
			top: "100px",
			right: "-10px",
			class: "before-covid-annot annot"
		},
	];
</script>

<div class="chart-container">
	<LayerCake
		padding={{ top: 170, right: 10, bottom: 20, left: 25 }}
		x={xKey}
		y={yKey}
		yNice={true}
		z={zKey}
		zScale={scaleOrdinal()}
		zRange={["#b6a391", "#444", "#999"]}
		yDomain={[0, null]}
		flatData={flatten(groupedData, "values")}
		data={groupedData}
	>
		<h2 class="title">Years of progress erased by the Pandemic</h2>
		<p class="subtitle">
			While pre-pandemic projections showed us coming up short of hitting the
			2030 target of eradicating poverty, <b>COVID-19 set progress back by as many
			as 4 years</b>.
		</p>
		<Svg>
			<AxisY
				textAnchor={"end"}
				dxTick={-4}
				dyTick={6}
				tickMarks={true}
				gridlines={false}
			/>
			<MultiLine />
      <line class="divider-2019"
        x1='62%'
        x2='62%'
        y1='0'
        y2='295'></line>
      <AxisX
        ticks={data.map(d => d[xKey])}
        yTick={28}
        snapTicks={true}
        gridlines={false}
      />
		</Svg>

    <Html>
      <Annotations {annotations} />
    </Html>
	</LayerCake>
</div>

<style>
	/*
      The wrapper div needs to have an explicit width and height in CSS.
      It can also be a flexbox child or CSS grid element.
      The point being it needs dimensions since the <LayerCake> element will
      expand to fill it.
    */
	.chart-container {
		width: 80%;
		height: 480px;
    margin: 80px auto;
	}

	.chart-container .title {
		font-size: 30px;
		line-height: 1.1;
		margin: 0 0 10px 0;
	}

	.chart-container .subtitle {
		width: 48ch;
		margin: 0;

		letter-spacing: -0.04em;
		line-height: 19px;
		font-size: 17px;
		color: #555;
		font-family: Inter;
		font-weight: 400;
	}

  .divider-2019{
    stroke: #222;
  }
  
	.chart-container :global(.layercake-annotation.annot) {
    text-align: right;
		width: 20ch;
		letter-spacing: -0.05em;
		line-height: 16px;
		font-size: 17px;
		color: #555;
		font-family: Inter;
		font-weight: 600;
	}

	.chart-container :global(.x-axis text) {
		font-size: 18px;
	}

	.chart-container :global(.x-axis .tick-0 text),
	.chart-container :global(.x-axis .tick-6 text) {
		color: #222;
		fill: #444;
		font-weight: bold;
	}

	.chart-container :global(.y-axis text) {
		font-size: 16px;
	}

	.chart-container :global(.line-group path) {
		stroke-width: 4;
	}

	.chart-container :global(.line-group path.line-2) {
		stroke-dasharray: 7;
	}
	/*
    .chart-container :global(.line-group path) {
      stroke: #555;
    } */
</style>
