<script>
	import { setContext } from "svelte";
	import { getMotion } from "./utils.js";
	import { themes } from "./config.js";
	import DataTeamHeader from "./layout/DataTeamHeader.svelte";
	import DataTeamFooter from "./layout/DataTeamFooter.svelte";
	import Header from "./layout/Header.svelte";
	import Section from "./layout/Section.svelte";
	import Media from "./layout/Media.svelte";
	import Scroller from "./layout/Scroller.svelte";
	import Filler from "./layout/Filler.svelte";
	import Divider from "./layout/Divider.svelte";
	import Table from "./layout/Table.svelte";
	import Arrow from "./ui/Arrow.svelte";
	import Toggle from "./ui/Toggle.svelte";
	import { writable } from "svelte/store";

	import { setColors } from "./utils.js";
	import { ScatterChart, BarChart } from "@onsvisual/svelte-charts";

	//Config color theme
	// Set theme globally (options are 'light', 'dark' or 'lightblue')
	let theme = "light";
	setContext("theme", theme);
	setColors(themes, theme);

	// DATA for Figures
	// Hardcoded, but should be able to load in data from e.g. csv's
	const genderGapData = [
		{ Group: "Females (w/in title)", Earnings: 0.994 },
		{ Group: "Females", Earnings: 0.835 },
		{ Group: "Males", Earnings: 1 },
	];
	const genderDecompData = [
		{
			Group: "ABHLO Females",
			id: "ABHLO Females1",
			Component: 0.141,
			Label: "85%",
		},
		{
			Group: "White Females",
			id: "White Females1",
			Component: 0.025,
			Label: "15%",
		},
		{
			Group: "Total Gap",
			id: "Total Gap1",
			Component: 0.165,
			Label: "$0.165",
		},
	];
	let highNonWhiteFemaleAgencyData = writable([
		{
			Agency: "Administration of Children's Services",
			"% ABHLO Female": "58.2%",
			"Median Salary": "$60k",
		},
		{
			Agency: "Human Resources Administration/Department of Social Services",
			"% ABHLO Female": "57.3%",
			"Median Salary": "$54k",
		},
		{
			Agency: "Department of Probation",
			"% ABHLO Female": "57.1%",
			"Median Salary": "$58k",
		},
		{
			Agency: "Department of Health and Mental Hygiene",
			"% ABHLO Female": "51.5%",
			"Median Salary": "$66k",
		},
		{
			Agency: "Law Department",
			"% ABHLO Female": "49.5%",
			"Median Salary": "$50k",
		},
	]);
	const salaryByFracNonWhiteFemaleData = [
		{ Share: 5, MedSalary: 92073, NumWorkers: 38604, Format: "$92k" },
		{ Share: 15, MedSalary: 85292, NumWorkers: 39897, Format: "$85k" },
		{ Share: 25, MedSalary: 92073, NumWorkers: 13079, Format: "$92k" },
		{ Share: 35, MedSalary: 78910, NumWorkers: 11025, Format: "$79k" },
		{ Share: 45, MedSalary: 65017, NumWorkers: 8078, Format: "$65k" },
		{ Share: 55, MedSalary: 61893, NumWorkers: 12194, Format: "$62k" },
		{ Share: 65, MedSalary: 51426.5, NumWorkers: 25832, Format: "$51k" },
		{ Share: 75, MedSalary: 58802, NumWorkers: 2607, Format: "$59k" },
		{ Share: 85, MedSalary: 56832, NumWorkers: 176, Format: "$57k" },
		{ Share: 95, MedSalary: 90000, NumWorkers: 101, Format: "$90k" },
	];

	// CONFIG FOR SCROLLER COMPONENTS
	let animation = getMotion(); // Set animation preference depending on browser preference

	// State variables for scrolly
	//INDEX is changed as you scroll, which then changes which part of figure is highlighted
	let genderGapFigIndex = 0;
	let genderGapFigSelect = null;
	$: {
		if (genderGapFigIndex == 0) {
			genderGapFigSelect = "Males";
		} else if (genderGapFigIndex == 1) {
			genderGapFigSelect = "Females";
		} else {
			genderGapFigSelect = "Females (w/in title)";
		}
	}
	let genderDecompFigIndex = 0;
	let genderDecompFigSelect = null;
	$: {
		if (genderDecompFigIndex == 0) {
			genderDecompFigSelect = "Total Gap1";
		} else if (genderDecompFigIndex == 1) {
			genderDecompFigSelect = "White Females1";
		} else {
			genderDecompFigSelect = "ABHLO Females1";
		}
	}
</script>

<!-- Add header to webpage -->
<DataTeamHeader filled={true} center={false} />

<!-- Add Title section to webpage -->
<Header
	bgcolor="#206095"
	bgfixed={true}
	theme="dark"
	center={false}
	short={true}
>
	<!-- Title -->
	<h1 style="font-family:Georgia">Pay Gaps in New York City</h1>
	<!-- Subtitle -->
	<p class="text-big" style="margin-top: 5px; font-family:Georgia">
		Closing the Gender and Racial Pay Gaps in the Municipal Workforce
	</p>
	<p class="text-small" style="margin-top: 50px; font-family:Georgia">
		Research by
		<a
			style="color:inherit"
			href="https://newyorkcitycouncil.github.io/web-dev-data-teams/#:~:text=Martinez"
			target="_blank">Rose Martinez</a
		>
	</p>
	<p class="text-small" style="margin-top: 50px; font-family:Georgia">
		June 2024
	</p>
	<!-- Toggle on/off animation button, better off for some browsers -->
	<p>
		<Toggle
			label="Animation {animation ? 'on' : 'off'}"
			mono={true}
			bind:checked={animation}
		/>
	</p>
	<div style="margin-top: 50px; font-family:Georgia">
		<Arrow color="white" {animation}></Arrow>
	</div>
</Header>

<!-- Distinctive block to draw attention to text, used here to set up article -->
<Filler theme="lightblue" short={true} wide={true} center={true}>
	<p class="text-big" style="font-family:Georgia">
		Large gender and racial pay gaps are present in the NYC municipal
		workforce.
	</p>
	<p></p>
	<p class="text-big" style="font-family:Georgia">
		Why is this the case? What steps can be taken to close these gaps?
	</p>
</Filler>

<!-- Place article text in Section blocks -->
<Section>
	<!-- Paragraph block -->
	<p>
		In the Data Team's "Pay Equity Law" <a
			style="color:inherit"
			href="http://council.nyc.gov/data/wp-content/uploads/sites/73/2024/04/PayDisparitiesReport2024.pdf"
			target="_blank">report</a
		>, we analyzed data from 2021 and found large pay gaps between male and
		female employees and white and Asian, Black or African American,
		Hispanic/Latino, or "Other race/ethnicity" (henceforth, "ABHLO")
		employees. These pay gaps are almost completely explained by
		occupational segregation. That is . . .
	</p>
</Section>

<!-- Visual divider -->
<Divider />

<Section>
	<!-- Header text for section title -->
	<h2>Pay Gaps in the Municipal Workforce</h2>

	<!-- Example of quote text -->
	<blockquote class="text-indent" style="margin-top:50px">
		"Large pay gaps between male and female and white and ABHLO employees
		are almost completely explained by occupational segregation."
	</blockquote>
</Section>

<!-- Example of scrolly section, see /src/Scroller.svelte for parameters you can pass,
see https://github.com/sveltejs/svelte-scroller for further detail. 
genderGapFigIndex changes as you scroll through and this changes which part
of figure is highlighted -->
<Scroller threshold="1" bind:index={genderGapFigIndex} splitscreen={true}>
	<!-- Place fixed portion of scrolly section (i.e. the figure) in
	the "background" slot -->
	<div slot="background">
		<figure>
			<div class="chart-ScrollFig">
				<!-- Example of bar chart, see 
				/node_modules/@onsvisual/svelte-charts/src/charts/BarChart.svelte
				for parameters you can pass in (if wish to change default value). 
				Important one for scrolly is "selected" -->
				<BarChart
					data={genderGapData}
					xKey="Earnings"
					yKey="Group"
					idKey="Group"
					padding={{ top: 0, bottom: 0, left: 60, right: 15 }}
					xPrefix="$"
					spacing="0.15"
					hover={true}
					selected={genderGapFigSelect}
					labels={true}
					labelKey="Earnings"
					prefixLabel="$"
					vertLabShift="40"
					xTicks={[0, 0.5, 1]}
					title="Gender Pay Gap"
				/>
			</div>
		</figure>
	</div>

	<!-- Scrolling parts of scrolly section (i.e. text) goes in foreground slot -->
	<div slot="foreground">
		<!-- Change the index when want to change which part of figure is highlighted -->
		<section genderGapFigIndex="1">
			<div class="col-medium">
				<p style="font-family:Georgia">
					To begin, let's examine the <b>gender pay gap</b>, where we
					use male employees as our reference group. In the figure,
					this is signified by male employees making a full dollar.
				</p>
			</div>
		</section>
		<section genderGapFigIndex="2">
			<div class="col-medium">
				<p style="font-family:Georgia">
					Female employees make less than males&mdash;on average,
					female employees earn $0.835 for every $1 that male
					employees make. In other words, there is a <b
						>$0.165 per dollar gender pay gap</b
					> in NYC's municipal workforce.
				</p>
			</div>
		</section>
		<section genderGapFigIndex="3">
			<div class="col-medium">
				<p style="font-family:Georgia">
					However, within the same job title, female employees earn
					more than $0.99 for every $1 that male employees make. Thus,
					occupational segregation almost completely explains the
					gender pay gap.
				</p>
			</div>
		</section>
	</div>
</Scroller>

<Divider />

<Section>
	<p>Example of Table below.</p>
</Section>

<!-- Use Media block as a wrapper around figures -->
<Media col="medium" caption="">
	<div class="chart-sml">
		<!-- Example of Table, see /layout/Table.svelte for parameter list -->
		<Table
			tableData={$highNonWhiteFemaleAgencyData}
			title="Agencies with Highest Shares of ABHLO Female Employees"
		/>
	</div>
</Media>

<Section>
	<p>Example of Scatter Chart below.</p>
</Section>

<Media col="medium" caption="">
	<div class="chart-sml">
		<!-- Example of scatter chart, see 
				/node_modules/@onsvisual/svelte-charts/src/charts/ScatterChart.svelte
				for parameter list -->
		<ScatterChart
			data={salaryByFracNonWhiteFemaleData}
			xKey="Share"
			xSuffix="%"
			xMax="101"
			yKey="MedSalary"
			yMin="20000"
			yMax="120000"
			yPrefix="$"
			rKey="NumWorkers"
			labelKey="Format"
			r={[2, 10]}
			xTicks={[0, 25, 50, 75, 100]}
			title="Job Title Median Salary by Share of ABHLO Female Employees"
			footer="Note: Circle size is proportional to number of 
			employees in each bin."
			labels={true}
			select={true}
			hover={true}
			padding={{ top: 5, bottom: 15, left: 90, right: 0 }}
		/>
	</div>
</Media>

<Divider />

<Section>
	<p>Second scrolly section example below</p>
</Section>

<Scroller threshold="1" bind:index={genderDecompFigIndex} splitscreen={true}>
	<div slot="background">
		<figure>
			<div class="chart-ScrollFig">
				<BarChart
					data={genderDecompData}
					xKey="Component"
					yKey="Group"
					idKey="id"
					padding={{ top: 0, bottom: 0, left: 60, right: 15 }}
					xPrefix="$"
					spacing="0.15"
					hover={true}
					selected={genderDecompFigSelect}
					labels={true}
					labelKey="Label"
					vertLabShift={40}
					title="Gender Pay Gap Decomposition"
				/>
			</div>
		</figure>
	</div>

	<div slot="foreground">
		<section genderDecompFigIndex="1">
			<div class="col-medium">
				<p style="font-family:Georgia">
					Let's reexamine the gender pay gap, where male employees are
					the reference group. Earlier, we saw that female employees
					earn $0.165 per dollar <i>less</i> than male employees on average.
				</p>
				<p style="font-family:Georgia">
					"Female employees" are made up of white and ABHLO female
					employees. How much of the gender pay gap is attributable to
					the lower wages of each of these two groups?
				</p>
			</div>
		</section>
		<section genderDecompFigIndex="2">
			<div class="col-medium">
				<p style="font-family:Georgia">
					We find that <b
						>white female employees account for $0.025 or 15%</b
					> of the gender pay gap. That is, if white female employees were
					paid the same as male employees (keeping ABHLO female employees'
					salaries as-is), the gender pay gap would be 15% smaller.
				</p>
			</div>
		</section>
		<section genderDecompFigIndex="3">
			<div class="col-medium">
				<p style="font-family:Georgia">
					That means that <b
						>ABHLO female employees account for $0.141 or 85%</b
					> of the gap. I.e., if ABHLO female employees earned the same
					as male employees, the gender pay gap would shrink by 85%.
				</p>
			</div>
		</section>
	</div>
</Scroller>

<!-- Add footer to webpage -->
<DataTeamFooter />

<!-- CSS STYLE CODE GOES HERE -->
<style>
	:global(svelte-scroller-foreground) {
		pointer-events: none !important;
	}
	:global(svelte-scroller-foreground section div) {
		pointer-events: all !important;
	}
	.chart-ScrollFig {
		height: 100%;
		width: 100%;
		padding-top: 30%;
		padding-left: 17%;
		padding-right: 10%;
		font-size: 0.8em;
	}
	.chart-sml {
		padding-left: 2%;
		padding-right: 2%;
		font-size: 0.8em;
	}
</style>
