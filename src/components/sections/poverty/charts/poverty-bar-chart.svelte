<script>
	import { LayerCake, Svg, Html } from "layercake";
	import { scaleBand } from "d3-scale";

	import data from "$data/poverty-data.csv";

	import Column from "$components/layercake/Column.svelte";
	import AxisX from "$components/layercake/AxisX.svelte";
	import AxisY from "$components/layercake/AxisY.svelte";
	import Annotations from "$components/layercake/Annotations.html.svelte";
	import Arrows from '$components/layercake/Arrows.svelte';

	const xKey = "year";
	const yKey = "value";

	// annotations config 
	const annotations = [
		{
			text: "In 2019, projections showed 6% of the population living in extreme poverty in 2030, <b>missing the target of eradicating poverty</b>",
			top: "-10%",
			right: "-1%",
			class: "poverty-annot",
			arrows: [
				{
					clockwise: true, // true or false, defaults to true
					source: {
						anchor: "middle-bottom", // can be `{left, middle, right},{top-middle-bottom}`
						dx: 40,
						dy: -10,
					},
					target: {
						x: "97%",
						y: "45%"
					}
				},
			]
		},
		{
			text: "Percentage of total population living on less than $1.90 a day",
			top: "-65px",
			left: "-25px",
			class: "poverty-ytitle"
		}
	];
	
	// convert to numbers
	data.forEach((d) => {
		d[yKey] = +d[yKey];
	});
</script>

<div class="chart-container">
	<h2 class="title">Global Poverty decreased drastically since 2010</h2>
	<p class="subtitle">
		While the number of people in extreme poverty decreased in the last decade,
		this progress slowed down towards 2019, only to have <b>several years of
		progress erased by the COVID-19 pandemic</b>.
	</p>

	<LayerCake
		padding= {{ top: 90, right: 0, bottom: 50, left: 20 }}
		x={xKey}
		xScale={scaleBand().paddingInner([0.02]).round(true)}
		xDomain={data.keys()}
		y={yKey}
		yDomain={[0, 16]}
		{data}
	>
		<Svg>
			<AxisY
				gridlines={true}
				ticks={[0, 4, 8, 12, 16]}
				textAnchor={"end"}
				xTick={15}
				yTick={9}
			/>
			<Column class="columns" />
			<AxisX gridlines={false} yTick={26} />
		</Svg>

		<Html>
			<Annotations {annotations} />
		</Html>
		
	  <Svg>
		<svelte:fragment slot="defs">
		</svelte:fragment>
		<Arrows {annotations}/>
	  </Svg> 
	
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
		width: 90%;
		height: 400px;
		margin-bottom: 170px; /* temp fix for overlapping issue */
	}

	.chart-container .title{
		font-size: 30px;
		line-height: 1.1;
		margin: 0 0 10px 0;
	}

	.chart-container .subtitle{
		max-width: 48ch;
		margin: 0;

		letter-spacing: -0.04em;
		line-height: 19px;
		font-size: 17px;
		color: #555;
		font-family: Inter;
		font-weight: 400;
	}

	.chart-container :global(rect:last-of-type) {
		fill: #333;
	}

	.chart-container :global(.x-axis .tick-0 text),
	.chart-container :global(.x-axis .tick-9 text) {
		color: #222;
		fill: #444;
		font-weight: bold;
	}

	.chart-container :global(.layercake-annotation.poverty-annot) {
		font-size: 15px;
		width: 28ch;
		color: #555;
		line-height: 1.2;
	}

	.chart-container :global(.swoops path) {
		stroke: #555;
    	stroke-dasharray: 4;
	}

	.chart-container :global(.layercake-annotation.poverty-ytitle) {
		width: 20ch;
		letter-spacing: -0.05em;
		line-height: 17px;
		font-size: 17px;
		color: #555;
		font-family: Inter;
		font-weight: 600;
	}

	/* Chart responsiveness for screens < 1400px wide */
	@media (max-width: 1400px) {
		.chart-container :global(.x-axis .tick:nth-of-type(even):not(:last-of-type) text) {
			visibility: hidden;
		}

		.chart-container .subtitle{
			width: 48ch;
			font-size: 18px;
			line-height: 1.1;
		}

		.chart-container .title{
			font-size: 22px;
			line-height: 1.1;
			letter-spacing: -1px;
		}
	}

	/*Responsive styling for tablets and smaller*/
	@media (max-width: 800px) {
		.chart-container :global(.x-axis .tick:not(:first-of-type):not(:last-of-type) text) {
			visibility: hidden;
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
		
		.chart-container :global(.layercake-annotation.poverty-annot) {
			top: -5% !important;
			font-size: 14px;
			width: 28ch;
			color: #555;
			line-height: 1;
		}

		.chart-container :global(rect) {
			stroke: #efefef;
			stroke-width: 1;
		}
	}
</style>
