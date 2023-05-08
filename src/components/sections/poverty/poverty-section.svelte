<script>
    import PovertyBarChart from "./charts/poverty-bar-chart.svelte";
    import PovertyBarChart2 from "./charts/poverty-bar-chart2.svelte";
    import PovertyProjectionLineChart from "./charts/poverty-projection-line-chart.svelte";
	import PovertyProjectionLineChart2 from "./charts/poverty-projection-line-chart2.svelte";
</script>

<div class="section poverty-section">
    <h1>Eradicating Poverty</h1>

    <p>
        Poverty is a complex issue that is affected by many factors, including
        economic growth, government policies, and demographic changes. In the
        last 10 years, there have been both positive and negative changes in
        poverty levels around the world.
    </p>
    <br>
    <p>
        On a global scale, the number of people living in extreme poverty
        (defined as living on less than $1.90 per day) has decreased. According
        to the World Bank, the number of people living in extreme poverty fell
        from around 1.9 billion in 2010 to around 700 million in 2020, which is
        a reduction of around 800 million people. This is partly due to economic
        growth in many developing countries, as well as government policies
        aimed at reducing poverty.
    </p>

    <br />

    <!-- <PovertyBarChart /> -->
    <PovertyBarChart2 />

    <p>
        The deceleration indicates that the world is not on track to achieve the
        target of less than 3 percent of the world living in extreme poverty by
        2030. The progress was good, but was still not enough, 2019 projections
        still showed us with 6% people living in extreme poverty in 2030.
        <br /><br />
        Poverty still remained a significant issue in many parts of the world,
        <b>particularly in sub-Saharan Africa and South Asia</b>. Despite being
        employed, 8% of the world's workers still lived in poverty - In
        sub-Saharan Africa the share of the working poor stood at 38%.
    </p>

    <!-- <PovertyRegionLineChart /> -->

    <h2>The Effect of the Global Pandemic</h2>
    <p>
        More than 4 years of progress against poverty has been erased by
        COVID-19. Covid reversed the steady progress of poverty reduction, which
        was further advanced due to rising inflation and the ongoing war in
        Ukraine. Around 75-95 million people additional people will will be
        living in extreme poverty as compared to pre-pandemic projections.
        <br />
        <!-- <PovertyProjectionLineChart /> -->
        <PovertyProjectionLineChart2 />
        <br />
        In 2020, for the first time in two decades, the world's share of workers
        living with their families below the international poverty line increased,
        from 6.7 per cent in 2019 to 7.2 per cent, meaning that
        <b>an additional 8 million workers were pushed into poverty</b>.
    </p>
</div>

<p />
