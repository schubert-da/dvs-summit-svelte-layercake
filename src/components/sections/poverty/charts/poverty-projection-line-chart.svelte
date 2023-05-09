<script>
	import { LayerCake, Svg, Html, groupLonger, flatten } from "layercake";

	import { scaleOrdinal } from "d3-scale";

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

	// convert data to numerical values from strings
	data.forEach(d => {
		seriesNames.forEach(name => {
			d[name] = +d[name];
		});
	});

	// annotations and title for y axis
	const annotations = [
		{
			text: "milllions of poor",
			top: "-10%",
			left: "-60px",
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
			top: "80px",
			right: "-10px",
			class: "before-covid-annot annot"
		},
	];
</script>

<div class="chart-container">
	<LayerCake
		padding={{ top: 200, right: 10, bottom: 20, left: 25 }}
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
		min-width: 300px;	
		max-width: 48ch;
		margin: 0;

		letter-spacing: -0.04em;
		line-height: 19px;
		font-size: 17px;
		color: #555;
		font-family: Inter;
		font-weight: 400;
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

	.chart-container :global(.layercake-annotation.poverty-annot) {
    	text-align: left;
	}

	.chart-container :global(.x-axis text) {
		font-size: 18px;
	}

	.chart-container :global(.x-axis g.tick:nth-child(even) text) { /* Hide every second tick label*/
		visibility: hidden !important;
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


	/* Styling for tablets and smaller */
	@media (max-width: 800px) {
		.chart-container{
			height: 480px;
			width: 90%;
		}

		.chart-container .subtitle{
			width: 30ch;
			font-size: 16px;
			line-height: 1.1;
		}

		.chart-container .title{
			font-size: 22px;
			line-height: 1.1;
			letter-spacing: -1px;
		}
		
		.chart-container :global(.layercake-annotation.annot) {
			line-height: 14px;
			font-size: 14px;
		}
	}
	
</style>
