<script>
	// CORE IMPORTS
	import { setContext, onMount } from "svelte";
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
	import { writable } from "svelte/store";

	// DEMO-SPECIFIC IMPORTS
	import {
		getData,
		setColors,
		getTopo,
		getBreaks,
		getColor,
	} from "./utils.js";
	import { colors, units } from "./config.js";
	import {
		ScatterChart,
		LineChart,
		BarChart,
	} from "@onsvisual/svelte-charts";

	// CORE CONFIG (COLOUR THEMES)
	// Set theme globally (options are 'light', 'dark' or 'lightblue')
	let theme = "light";
	setContext("theme", theme);
	setColors(themes, theme);

	// CONFIG FOR SCROLLER COMPONENTS
	// Config
	const threshold = 0.65;
	// State
	let animation = getMotion(); // Set animation preference depending on browser preference
	let id = {}; // Object to hold visible section IDs of Scroller components
	let idPrev = {}; // Object to keep track of previous IDs, to compare for changes
	onMount(() => {
		idPrev = { ...id };
	});

	// Data for Figures (in future can figure out how to load in data from e.g. csv's)
	// gender pay gap
	const genderGapData = [
		{ Group: "Females (w/in title)", Earnings: 0.994 },
		{ Group: "Females", Earnings: 0.835 },
		{ Group: "Males", Earnings: 1 },
	];
	// racial pay gap
	const raceGapData = [
		{ Group: "ABHLO (w/in title)", Earnings: 0.992 },
		{ Group: "ABHLO", Earnings: 0.842 },
		{ Group: "White", Earnings: 1 },
	];
	// combined pay gap
	const combinedGapData = [
		{ Group: "All Others (w/in title)", Earnings: 0.987 },
		{ Group: "All Others", Earnings: 0.815 },
		{ Group: "White Males", Earnings: 1 },
	];
	// gender gap decomposition
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
	// racial gap decomposition
	const raceDecompData = [
		{
			Group: "ABHLO Females",
			id: "ABHLO Females2",
			Component: 0.107,
			Label: "67.6%",
		},
		{
			Group: "ABHLO Males",
			id: "ABHLO Males2",
			Component: 0.051,
			Label: "32.4%",
		},
		{
			Group: "Total Gap",
			id: "Total Gap2",
			Component: 0.158,
			Label: "$0.158",
		},
	];
	// combined gap decomposition
	const combinedDecompData = [
		{
			Group: "ABHLO Females",
			id: "ABHLO Females3",
			Component: 0.103,
			Label: "55.4%",
		},
		{
			Group: "ABHLO Males",
			id: "ABHLO Males3",
			Component: 0.058,
			Label: "31.5%",
		},
		{
			Group: "White Females",
			id: "White Females3",
			Component: 0.024,
			Label: "13.1%",
		},
		{
			Group: "Total Gap",
			id: "Total Gap3",
			Component: 0.185,
			Label: "$0.185",
		},
	];

	//Data for Tables
	/*let lowNonWhiteFemaleData = writable([
		{
			"Job Title": "Caseworker",
			Agency: "HRA/DSS",
			"% Non-White": "81%",
			"% Female": "75%",
			"Median Salary": "$48k",
			"Count": "1,222",
		},
		{
			"Job Title": "Clerical Associate",
			Agency: "DFTA",
			"% Non-White": "82%",
			"% Female": "82%",
			"Median Salary": "$43k",
			"Count": "3,222",
		},
		{
			"Job Title": "Community Assistant",
			Agency: "OATH",
			"% Non-White": "82%",
			"% Female": "59%",
			"Median Salary": "$37k",
			"Count": "765",
		},
		{
			"Job Title": "Community Associate",
			Agency: "HRA/DSS",
			"% Non-White": "80%",
			"% Female": "74%",
			"Median Salary": "$46k",
			"Count": "3,425",
		},
		{
			"Job Title": "Eligibility Specialist",
			Agency: "HRA/DSS",
			"% Non-White": "83%",
			"% Female": "82%",
			"Median Salary": "$43k",
			"Count": "2,068",
		},
		{
			"Job Title": "Job Opportunity Specialist",
			Agency: "HRA/DSS",
			"% Non-White": "77%",
			"% Female": "70%",
			"Median Salary": "$49k",
			"Count": "1,184",
		},
		{
			"Job Title": "Police Administrative Aide",
			Agency: "NYPD",
			"% Non-White": "73%",
			"% Female": "92%",
			"Median Salary": "$42k",
			"Count": "1,159",
		},

		{
			"Job Title": "Police Communications Tech.",
			Agency: "NYPD",
			"% Non-White": "75%",
			"% Female": "85%",
			"Median Salary": "$53k",
			"Count": "1,530",
		},
		{
			"Job Title": "Probation Officer",
			Agency: "DOP",
			"% Non-White": "85%",
			"% Female": "72%",
			"Median Salary": "$54k",
			"Count": "570",
		},
		{
			"Job Title": "School Safety Agent",
			Agency: "NYPD",
			"% Non-White": "82%",
			"% Female": "72%",
			"Median Salary": "$50k",
			"Count": "4,444",
		},
	]);*/
	// 17 low-paying non-white dominated jobs
	/*let lowNonWhiteData = writable([
		{
			"Job Title": "Assoc. Job Opportunity Specialist",
			Agency: "HRA/DSS",
			"% Non-White": "87%",
			"% Female": "78%",
			"Median Salary": "$60k",
			"Count": "687",
		},
		{
			"Job Title": "Caseworker",
			Agency: "HRA/DSS",
			"% Non-White": "81%",
			"% Female": "75%",
			"Median Salary": "$48k",
			"Count": "1,222",
		},
		{
			"Job Title": "Clerical Associate",
			Agency: "DFTA",
			"% Non-White": "82%",
			"% Female": "82%",
			"Median Salary": "$43k",
			"Count": "3,222",
		},
		{
			"Job Title": "Community Assistant",
			Agency: "OATH",
			"% Non-White": "82%",
			"% Female": "59%",
			"Median Salary": "$37k",
			"Count": "765",
		},
		{
			"Job Title": "Community Associate",
			Agency: "HRA/DSS",
			"% Non-White": "80%",
			"% Female": "74%",
			"Median Salary": "$46k",
			"Count": "3,425",
		},
		{
			"Job Title": "EMS-EMT",
			Agency: "FDNY",
			"% Non-White": "51%",
			"% Female": "25%",
			"Median Salary": "$49k",
			"Count": "3,039",
		},
		{
			"Job Title": "Eligibility Specialist",
			Agency: "HRA/DSS",
			"% Non-White": "83%",
			"% Female": "82%",
			"Median Salary": "$43k",
			"Count": "2,068",
		},
		{
			"Job Title": "Fraud Investigator",
			Agency: "DOF",
			"% Non-White": "85%",
			"% Female": "60%",
			"Median Salary": "$59k",
			"Count": "549",
		},
		{
			"Job Title": "Job Opportunity Specialist",
			Agency: "HRA/DSS",
			"% Non-White": "77%",
			"% Female": "70%",
			"Median Salary": "$49k",
			"Count": "1,184",
		},
		{
			"Job Title": "Police Administrative Aide",
			Agency: "NYPD",
			"% Non-White": "73%",
			"% Female": "92%",
			"Median Salary": "$42k",
			"Count": "1,159",
		},

		{
			"Job Title": "Police Communications Tech.",
			Agency: "NYPD",
			"% Non-White": "75%",
			"% Female": "85%",
			"Median Salary": "$53k",
			"Count": "1,530",
		},
		{
			"Job Title": "Probation Officer",
			Agency: "DOP",
			"% Non-White": "85%",
			"% Female": "72%",
			"Median Salary": "$54k",
			"Count": "570",
		},
		{
			"Job Title": "School Safety Agent",
			Agency: "NYPD",
			"% Non-White": "82%",
			"% Female": "72%",
			"Median Salary": "$50k",
			"Count": "4,444",
		},
		{
			"Job Title": "Senior Police Admin. Aide",
			Agency: "NYPD",
			"% Non-White": "77%",
			"% Female": "95%",
			"Median Salary": "$54k",
			"Count": "685",
		},
		{
			"Job Title": "Special Officer",
			Agency: "DCAS",
			"% Non-White": "80%",
			"% Female": "49%",
			"Median Salary": "$41k",
			"Count": "753",
		},
		{
			"Job Title": "Traffic Enforcement Agent",
			Agency: "NYPD",
			"% Non-White": "81%",
			"% Female": "39%",
			"Median Salary": "$43k",
			"Count": "2,763",
		},
		{
			"Job Title": "Youth Development Specialist",
			Agency: "ACS",
			"% Non-White": "87%",
			"% Female": "32%",
			"Median Salary": "$49k",
			"Count": "609",
		},
	]);*/
	// mid-paying male dominated jobs
	let midMaleData = writable([
		{
			"Job Title": "Auto Mechanic",
			Agency: "NYPD",
			"% Female": "0.3%",
			"Median Salary": "$91k",
			Count: "791",
		},
		{
			"Job Title": "Computer Specialist",
			Agency: "DOHMH",
			"% Female": "33.4%",
			"Median Salary": "$106k",
			Count: "1,095",
		},
		{
			"Job Title": "Correction Officer",
			Agency: "DOC",
			"% Female": "43.1%",
			"Median Salary": "$92k",
			Count: "6,885",
		},
		{
			"Job Title": "EMS-Paramedic",
			Agency: "FDNY",
			"% Female": "30.7%",
			"Median Salary": "$73k",
			Count: "916",
		},
		{
			"Job Title": "Firefighter",
			Agency: "FDNY",
			"% Female": "1.5%",
			"Median Salary": "$92k",
			Count: "8,101",
		},
		{
			"Job Title": "Highway Repairer",
			Agency: "DOT",
			"% Female": "6.4%",
			"Median Salary": "$99k",
			Count: "452",
		},
		{
			"Job Title": "Police Officer",
			Agency: "NYPD",
			"% Female": "21.5%",
			"Median Salary": "$85k",
			Count: "23,536",
		},
		{
			"Job Title": "Sanitation Worker",
			Agency: "DSNY",
			"% Female": "3.1%",
			"Median Salary": "$83k",
			Count: "6,602",
		},
		{
			"Job Title": "Sewage Treatment Worker",
			Agency: "DEP",
			"% Female": "0.2%",
			"Median Salary": "$94k",
			Count: "555",
		},
	]);
	// high-paying male dominated jobs
	let highMaleData = writable([
		{
			"Job Title": "Fire Captain",
			Agency: "FDNY",
			"% Female": "0.2%",
			"Median Salary": "$136k",
			Count: "557",
		},
		{
			"Job Title": "Fire Lieutenant",
			Agency: "FDNY",
			"% Female": "0.6%",
			"Median Salary": "$118k",
			Count: "1,452",
		},
		{
			"Job Title": "Police Lieutenant",
			Agency: "NYPD",
			"% Female": "12.4%",
			"Median Salary": "$136k",
			Count: "1,352",
		},
		{
			"Job Title": "P.O. DA Det. GR3",
			Agency: "NYPD",
			"% Female": "15%",
			"Median Salary": "$105k",
			Count: "3,351",
		},
		{
			"Job Title": "P.O. Det Spec.",
			Agency: "NYPD",
			"% Female": "19.6%",
			"Median Salary": "$105k",
			Count: "949",
		},
		{
			"Job Title": "P.O. D/A Det. 2nd GR",
			Agency: "NYPD",
			"% Female": "17.1%",
			"Median Salary": "$118k",
			Count: "690",
		},
		{
			"Job Title": "Police Sergeant",
			Agency: "NYPD",
			"% Female": "16.6%",
			"Median Salary": "$118k",
			Count: "3,720",
		},
		{
			"Job Title": "Supervising EMS",
			Agency: "FDNY",
			"% Female": "26.1%",
			"Median Salary": "$80k",
			Count: "491",
		},
		{
			"Job Title": "Sanitation Supervisor",
			Agency: "DSNY",
			"% Female": "5.8%",
			"Median Salary": "$112k",
			Count: "1,097",
		},
	]);
	// mid-paying white dominated jobs
	let midWhiteData = writable([
		{
			"Job Title": "Agency Attorney",
			Agency: "DCAS",
			"% ABHLO": "38%",
			"Median Salary": "$92k",
			Count: "779",
		},
		{
			"Job Title": "Auto Mechanic",
			Agency: "NYPD",
			"% ABHLO": "46.8%",
			"Median Salary": "$91k",
			Count: "791",
		},
		{
			"Job Title": "Criminalist",
			Agency: "DOHMH",
			"% ABHLO": "45.7%",
			"Median Salary": "$82k",
			Count: "451",
		},
		{
			"Job Title": "EMS-Paramedic",
			Agency: "FDNY",
			"% ABHLO": "52.5%",
			"Median Salary": "$73k",
			Count: "916",
		},
		{
			"Job Title": "Firefighter",
			Agency: "FDNY",
			"% ABHLO": "25.4%",
			"Median Salary": "$92k",
			Count: "8,101",
		},
		{
			"Job Title": "Occupational Therapist",
			Agency: "DOE",
			"% ABHLO": "39.6%",
			"Median Salary": "$81k",
			Count: "2,091",
		},
		{
			"Job Title": "Police Officer",
			Agency: "NYPD",
			"% ABHLO": "52.6%",
			"Median Salary": "$85k",
			Count: "23,536",
		},
		{
			"Job Title": "Sanitation Worker",
			Agency: "DSNY",
			"% ABHLO": "45.3%",
			"Median Salary": "$83k",
			Count: "6,602",
		},
		{
			"Job Title": "Sewage Treatment Worker",
			Agency: "DEP",
			"% ABHLO": "35.9%",
			"Median Salary": "$94k",
			Count: "555",
		},
	]);
	// high-paying white dominated jobs
	let highWhiteData = writable([
		{
			"Job Title": "Fire Captain",
			Agency: "FDNY",
			"% ABHLO": "7.2%",
			"Median Salary": "$136k",
			Count: "557",
		},
		{
			"Job Title": "Fire Lieutenant",
			Agency: "FDNY",
			"% ABHLO": "12.7%",
			"Median Salary": "$118k",
			Count: "1,452",
		},
		{
			"Job Title": "Police Lieutenant",
			Agency: "NYPD",
			"% ABHLO": "44%",
			"Median Salary": "$136k",
			Count: "1,352",
		},
		{
			"Job Title": "P.O. DA Det. GR3",
			Agency: "NYPD",
			"% ABHLO": "42.9%",
			"Median Salary": "$105k",
			Count: "3,351",
		},
		{
			"Job Title": "P.O. Det Spec.",
			Agency: "NYPD",
			"% ABHLO": "45.8%",
			"Median Salary": "$105k",
			Count: "949",
		},
		{
			"Job Title": "P.O. D/A Det. 2nd GR",
			Agency: "NYPD",
			"% ABHLO": "47.1%",
			"Median Salary": "$118k",
			Count: "690",
		},
		{
			"Job Title": "Police Sergeant",
			Agency: "NYPD",
			"% ABHLO": "44.7%",
			"Median Salary": "$118k",
			Count: "3,720",
		},
		{
			"Job Title": "Supervising EMS",
			Agency: "FDNY",
			"% ABHLO": "46.8%",
			"Median Salary": "$80k",
			Count: "491",
		},
		{
			"Job Title": "Sanitation Supervisor",
			Agency: "DSNY",
			"% ABHLO": "38.8%",
			"Median Salary": "$112k",
			Count: "1,097",
		},
	]);
	// agencies with highest share of ABHLO females
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
	// agencies with lowest share of ABHLO females
	let lowNonWhiteFemaleAgencyData = writable([
		{
			Agency: "Department of Environmental Protection",
			"% ABHLO Female": "15.6%",
			"Median Salary": "$86k",
		},
		{
			Agency: "Department of Transportation",
			"% ABHLO Female": "19.2%",
			"Median Salary": "$78k",
		},
		{
			Agency: "Technology and Innovation",
			"% ABHLO Female": "22.1%",
			"Median Salary": "$95k",
		},
		{
			Agency: "Department of Sanitation",
			"% ABHLO Female": "5.6%",
			"Median Salary": "$83k",
		},
		{
			Agency: "Fire Department",
			"% ABHLO Female": "6.7%",
			"Median Salary": "$92k",
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

	// State variables for scrolly
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
	let raceGapFigIndex = 0;
	let raceGapFigSelect = null;
	$: {
		if (raceGapFigIndex == 0) {
			raceGapFigSelect = "White";
		} else if (raceGapFigIndex == 1) {
			raceGapFigSelect = "ABHLO";
		} else {
			raceGapFigSelect = "ABHLO (w/in title)";
		}
	}
	let combinedGapFigIndex = 0;
	let combinedGapFigSelect = null;
	$: {
		if (combinedGapFigIndex == 0) {
			combinedGapFigSelect = "White Males";
		} else if (combinedGapFigIndex == 1) {
			combinedGapFigSelect = "All Others";
		} else {
			combinedGapFigSelect = "All Others (w/in title)";
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
	let raceDecompFigIndex = 0;
	let raceDecompFigSelect = null;
	$: {
		if (raceDecompFigIndex == 0) {
			raceDecompFigSelect = "Total Gap2";
		} else if (raceDecompFigIndex == 1) {
			raceDecompFigSelect = "ABHLO Males2";
		} else {
			raceDecompFigSelect = "ABHLO Females2";
		}
	}
	let combinedDecompFigIndex = 0;
	let combinedDecompFigSelect = null;
	$: {
		if (combinedDecompFigIndex == 0) {
			combinedDecompFigSelect = "Total Gap3";
		} else if (combinedDecompFigIndex == 1) {
			combinedDecompFigSelect = "White Females3";
		} else if (combinedDecompFigIndex == 2) {
			combinedDecompFigSelect = "ABHLO Males3";
		} else {
			combinedDecompFigSelect = "ABHLO Females3";
		}
	}

	// Functions for on:select and on:hover events
	function doSelect(e) {
		selected = e.detail.id;
	}
	function doHover(e) {
		hovered = e.detail.id;
	}
</script>

<DataTeamHeader filled={true} center={false} />

<Header
	bgcolor="#206095"
	bgfixed={true}
	theme="dark"
	center={false}
	short={true}
>
	<h1 style="font-family:Georgia">Pay Gaps in New York City</h1>
	<p class="text-big" style="margin-top: 5px; font-family:Georgia">
		Closing the Gender and Racial Pay Gaps in the Municipal Workforce
	</p>
	<p class="text-small" style="margin-top: 50px; font-family:Georgia">
		Research by <a
			style="color:inherit"
			href="https://www.linkedin.com/in/julia-fredenburg/"
			target="_blank">Julia Fredenburg</a
		>,
		<a
			style="color:inherit"
			href="https://newyorkcitycouncil.github.io/web-dev-data-teams/#:~:text=Hirota"
			target="_blank">Reese Hirota</a
		>,
		<a
			style="color:inherit"
			href="https://newyorkcitycouncil.github.io/web-dev-data-teams/#:~:text=Koepcke"
			target="_blank">Eric J. Koepcke</a
		>,
		<a
			style="color:inherit"
			href="https://newyorkcitycouncil.github.io/web-dev-data-teams/#:~:text=Martinez"
			target="_blank">Rose Martinez</a
		>,
		<a
			style="color:inherit"
			href="https://newyorkcitycouncil.github.io/web-dev-data-teams/#:~:text=Melissa"
			target="_blank">Melissa Nu&ntilde;ez</a
		>,
		<a
			style="color:inherit"
			href="https://newyorkcitycouncil.github.io/web-dev-data-teams/#:~:text=Wu"
			target="_blank">James Wu</a
		>, and
		<a
			style="color:inherit"
			href="https://www.linkedin.com/in/christopher-zawora-56702168/"
			target="_blank">Christopher Zawora</a
		>
	</p>
	<p class="text-small" style="margin-top: 10px; font-family:Georgia">
		Webpage by <a
			style="color:inherit"
			href="https://newyorkcitycouncil.github.io/web-dev-data-teams/#:~:text=Koepcke"
			target="_blank">Eric J. Koepcke</a
		>
	</p>
	<p class="text-small" style="margin-top: 50px; font-family:Georgia">
		April 2024
	</p>
	<!-- <p>
		<Toggle
			label="Animation {animation ? 'on' : 'off'}"
			mono={true}
			bind:checked={animation}
		/>
	</p> -->
	<div style="margin-top: 50px; font-family:Georgia">
		<Arrow color="white" {animation}></Arrow>
	</div>
</Header>

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

<Section>
	<p>
		In 2019, the New York City Council passed Local Law 18, also known as
		the “Pay Equity Law,” to identify pay disparities among employees of the
		City of New York based on race/ethnicity and gender. The law provides
		the Council's Data Team with access to municipal employee data to
		facilitate investigation of any pay disparities.
	</p>
	<p>
		In the Data Team's "Pay Equity Law" <a
			style="color:inherit"
			href="http://council.nyc.gov/data/wp-content/uploads/sites/73/2024/04/PayDisparitiesReport2024.pdf"
			target="_blank">report</a
		>, we analyzed data from 2021 and found large pay gaps between male and
		female employees and white and Asian, Black or African American,
		Hispanic/Latino, or "Other race/ethnicity" (henceforth, "ABHLO")
		employees. These pay gaps are almost completely explained by
		occupational segregation. That is, within the same job title, there is
		essentially equal pay for all individuals regardless of gender and race.
		However, the uneven distribution of employees by race and gender across
		occupations and the pay differences between these occupations lead to
		large Citywide gender and race pay gaps. Expanding upon this, we show
		how segregation both across and within City agencies contributes to pay
		gaps and how the particularly low pay received by ABHLO female employees
		explains large portions of these pay gaps.
	</p>
	<p>
		The City's pay gaps are not due to a few problematic agencies or titles,
		rather they are the result of occupational segregation across the City's
		workforce. One way to address these issues is to promote greater
		diversity in well-paid career tracks that are currently predominantly
		held by male and white employees. Via simulations, we show that if such
		careers are diversified over time the City's gender and racial pay gaps
		will significantly narrow. To make progress on diversification, the
		Council proposes distributing workplace culture surveys to gain a deeper
		understanding of why occupational segregation exists and how to best
		address it.
	</p>
	<p>
		The Council proposes career counseling services to assist municipal
		employees in lower-paying jobs, which are currently predominantly held
		by female and ABHLO employees. Counseling on professional growth and
		promotional opportunities would help employees to move up their current
		career ladder or to move to better-paying municipal careers.
		Additionally, the Council looks forward to the results of the City's
		forthcoming comparable worth analysis, which will assess whether
		municipal titles are being paid equitably and could potentially help
		narrow the City's pay gaps.
	</p>
</Section>

<Divider />

<Section>
	<h2>Pay Gaps in the Municipal Workforce</h2>

	<blockquote class="text-indent" style="margin-top:50px">
		"Large pay gaps between male and female and white and ABHLO employees
		are almost completely explained by occupational segregation."
	</blockquote>
</Section>

<Section>
	<p>
		The Data Team compared the average salaries for male and female and for
		white and ABHLO employees in order to calculate the Citywide gender and
		racial pay gaps. The findings from these comparisons are presented
		below. Additionally, we show that occupational segregation by gender and
		race almost completely explains these pay gaps.
	</p>
	<p style="color:#949494">
		Note: Below we use the arithmetic mean to calculate average salaries.
		Past Data Team "Pay Equity" reports have used median salary-based pay
		gaps. See the full <a
			style="color:inherit"
			href="http://council.nyc.gov/data/wp-content/uploads/sites/73/2024/04/PayDisparitiesReport2024.pdf"
			target="_blank">report</a
		> for the median-based pay gaps.
	</p>
</Section>

<Scroller threshold="1" bind:index={genderGapFigIndex} splitscreen={true}>
	<div slot="background">
		<figure>
			<div class="chart-ScrollFig">
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

	<div slot="foreground">
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

<Scroller threshold="1" bind:index={raceGapFigIndex} splitscreen={true}>
	<div slot="background">
		<figure>
			<div class="chart-ScrollFig">
				<BarChart
					data={raceGapData}
					xKey="Earnings"
					yKey="Group"
					idKey="Group"
					padding={{ top: 0, bottom: 0, left: 60, right: 15 }}
					xPrefix="$"
					spacing="0.15"
					hover={true}
					selected={raceGapFigSelect}
					labels={true}
					labelKey="Earnings"
					prefixLabel="$"
					vertLabShift={40}
					xTicks={[0, 0.5, 1]}
					title="Racial Pay Gap"
				/>
			</div>
		</figure>
	</div>

	<div slot="foreground">
		<section raceGapFigIndex="1">
			<div class="col-medium">
				<p style="font-family:Georgia">
					Now, let's look at the <b>racial pay gap</b>, where we use
					white employees as our reference group. This is signified in
					the figure by white employees making a full dollar.
				</p>
			</div>
		</section>
		<section raceGapFigIndex="2">
			<div class="col-medium">
				<p style="font-family:Georgia">
					ABHLO employees make less than white employees&mdash;on
					average, ABHLO employees earn $0.842 for every $1 that white
					employees make. I.e., there is a <b
						>$0.158 per dollar racial pay gap</b
					>.
				</p>
			</div>
		</section>
		<section raceGapFigIndex="3">
			<div class="col-medium">
				<p style="font-family:Georgia">
					As with gender, within the same job title, ABHLO employees
					earn more than $0.99 for every $1 that white employees make.
					I.e., occupational segregation almost completely explains
					the racial pay gap.
				</p>
			</div>
		</section>
	</div>
</Scroller>

<Divider />

<Scroller threshold="1" bind:index={combinedGapFigIndex} splitscreen={true}>
	<div slot="background">
		<figure>
			<div class="chart-ScrollFig">
				<BarChart
					data={combinedGapData}
					xKey="Earnings"
					yKey="Group"
					idKey="Group"
					padding={{ top: 0, bottom: 0, left: 60, right: 15 }}
					xPrefix="$"
					spacing="0.15"
					hover={true}
					selected={combinedGapFigSelect}
					labels={true}
					labelKey="Earnings"
					prefixLabel="$"
					vertLabShift={40}
					xTicks={[0, 0.5, 1]}
					title="Combined Pay Gap"
				/>
			</div>
		</figure>
	</div>

	<div slot="foreground">
		<section combinedGapFigIndex="1">
			<div class="col-medium">
				<p style="font-family:Georgia">
					Finally, we combine race and gender to look at the <b
						>"combined" pay gap</b
					>, where we use white male employees as our reference group.
					This is signified in the figure by white male employees
					making a full dollar.
				</p>
			</div>
		</section>
		<section combinedGapFigIndex="2">
			<div class="col-medium">
				<p style="font-family:Georgia">
					White male employees make more than others&mdash;on average,
					all other employees earn $0.815 for every $1 that white male
					employees make. I.e., there is a <b
						>$0.185 per dollar combined pay gap</b
					>.
				</p>
			</div>
		</section>
		<section combinedGapFigIndex="3">
			<div class="col-medium">
				<p style="font-family:Georgia">
					As above, this pay gap is due to occupational segregation.
					Within the same job title, all other employees earn about
					$0.99 for every $1 that white male employees make.
				</p>
			</div>
		</section>
	</div>
</Scroller>

<Divider />

<Section>
	<p>
		To understand these pay gaps better, let's zoom in on the City's
		agencies. In the table below, we list the five agencies with the highest
		shares of ABHLO female employees. The median salary for the City's
		workforce is $76k and we can see that all five of these agencies have
		median salaries below the City's median. The reverse pattern emerges
		when considering the five agencies with the lowest shares of ABHLO
		female employees&mdash;those agencies all have median salaries above the
		City’s median.
	</p>
</Section>

<Media col="medium" caption="">
	<div class="chart-sml">
		<Table
			tableData={$highNonWhiteFemaleAgencyData}
			title="Agencies with Highest Shares of ABHLO Female Employees"
		/>
	</div>
</Media>
<Media col="medium" caption="">
	<div class="chart-sml">
		<Table
			tableData={$lowNonWhiteFemaleAgencyData}
			title="Agencies with Lowest Shares of ABHLO Female Employees"
		/>
	</div>
</Media>

<Section>
	<p>
		This is a general pattern&mdash;<b
			>across agencies, as the share of ABHLO female employees rises,
			median salary tends to fall</b
		>. Segregation and pay disparities across agencies contribute to the
		Citywide pay gaps we saw above.
	</p>
	<p>
		However, segregation is not only an <i>across-agency</i> issue, it is
		also a problem
		<i>within</i> agencies. As can be seen in the figure below,
		<b
			>job titles with higher shares of ABHLO female employees tend to be
			paid less</b
		>, with some exceptions among select high-ranking positions, which
		employ very few people.
	</p>
	<!-- <p>
		The figure below visualizes this relationship. It plots a job title's
		median salary by the title's share of non-white female employees. The
		horizontal axis is binned&mdash;from left-to-right, the bins are 0-10%,
		10-20%, . . ., 90-100%. Many employees work in titles with a 0-20%
		non-white female employee share and, on average, these titles have
		median salaries of $85k or higher. In contrast, the next largest group
		is titles with a 60-70% non-white female employee share and, on average,
		these titles only have a median salary of $51k.
	</p> -->
</Section>

<Media col="medium" caption="">
	<div class="chart-sml">
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
	<blockquote class="text-indent" style="margin-top:50px">
		"The City's pay gaps largely result from the low salaries paid to ABHLO
		female employees."
	</blockquote>

	<p>
		Although the above analysis focused on ABHLO female employees, it also
		applies to female and ABHLO employees separately. The results are more
		stark for ABHLO female employees, however. This is because ABHLO female
		employees are concentrated in particularly low-paying roles, even
		relative to white female and ABHLO male employees.
	</p>
	<p>
		In fact, the City's pay gaps largely result from the low salaries paid
		to ABHLO female employees. We show this below by decomposing how white
		female, ABHLO female, and ABHLO male employees separately contribute to
		the City's pay gaps.
	</p>
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

<Divider />

<Scroller threshold="1" bind:index={raceDecompFigIndex} splitscreen={true}>
	<div slot="background">
		<figure>
			<div class="chart-ScrollFig">
				<BarChart
					data={raceDecompData}
					xKey="Component"
					yKey="Group"
					idKey="id"
					padding={{ top: 0, bottom: 0, left: 60, right: 15 }}
					xPrefix="$"
					spacing="0.15"
					hover={true}
					selected={raceDecompFigSelect}
					labels={true}
					labelKey="Label"
					vertLabShift={40}
					title="Racial Pay Gap Decomposition"
				/>
			</div>
		</figure>
	</div>

	<div slot="foreground">
		<section raceDecompFigIndex="1">
			<div class="col-medium">
				<p style="font-family:Georgia">
					Now, let's return to the racial pay gap, where white
					employees are the reference group. We saw that ABHLO
					employees earn $0.158 per dollar <i>less</i> than white employees
					on average.
				</p>
				<p style="font-family:Georgia">
					"ABHLO employees" are made up of ABHLO male and ABHLO female
					employees. How much of the racial pay gap is attributable to
					the lower wages of each of these two groups?
				</p>
			</div>
		</section>
		<section raceDecompFigIndex="2">
			<div class="col-medium">
				<p style="font-family:Georgia">
					We find that <b
						>ABHLO male employees account for $0.051 or 32.4%</b
					> of the racial pay gap. That is, if ABHLO male employees were
					paid the same as white employees (keeping ABHLO female employees'
					salaries as-is), the racial pay gap would be one-third smaller.
				</p>
			</div>
		</section>
		<section raceDecompFigIndex="3">
			<div class="col-medium">
				<p style="font-family:Georgia">
					<b>ABHLO female employees account for $0.107 or 67.6%</b> of
					the gap. I.e., if ABHLO female employees earned the same as white
					employees, the racial pay gap would shrink by two-thirds.
				</p>
			</div>
		</section>
	</div>
</Scroller>

<Divider />

<Scroller threshold="1" bind:index={combinedDecompFigIndex} splitscreen={true}>
	<div slot="background">
		<figure>
			<div class="chart-ScrollFig">
				<BarChart
					data={combinedDecompData}
					xKey="Component"
					yKey="Group"
					idKey="id"
					padding={{ top: 0, bottom: 0, left: 60, right: 15 }}
					xPrefix="$"
					spacing="0.15"
					hover={true}
					selected={combinedDecompFigSelect}
					labels={true}
					labelKey="Label"
					vertLabShift={30}
					title="Combined Pay Gap Decomposition"
				/>
			</div>
		</figure>
	</div>

	<div slot="foreground">
		<section combinedDecompFigIndex="1">
			<div class="col-medium">
				<p style="font-family:Georgia">
					Finally, let's look at the "combined" pay gap, where white
					male employees are the reference group. Earlier, we saw that
					all other employees earn $0.185 per dollar <i>less</i> than white
					male employees on average.
				</p>
				<p style="font-family:Georgia">
					"All other employees" include white female, ABHLO male, and
					ABHLO female employees. How much of the combined pay gap is
					attributable to the lower wages of each of these three
					groups?
				</p>
			</div>
		</section>
		<section combinedDecompFigIndex="2">
			<div class="col-medium">
				<p style="font-family:Georgia">
					We find that <b
						>white female employees account for $0.024 or 13.1%</b
					> of the combined pay gap. That is, if white female employees
					were paid the same as white male employees (keeping ABHLO male
					and female employees' salaries as-is), the combined pay gap would
					be 13.1% smaller.
				</p>
			</div>
		</section>
		<section combinedDecompFigIndex="3">
			<div class="col-medium">
				<p style="font-family:Georgia">
					<b>ABHLO male employees account for $0.058 or 31.5%</b> of the
					gap. I.e., if ABHLO male employees earned the same as white male
					employees, the combined pay gap would shrink by 31.5%.
				</p>
			</div>
		</section>
		<section combinedDecompFigIndex="4">
			<div class="col-medium">
				<p style="font-family:Georgia">
					Therefore, <b
						>ABHLO female employees account for $0.103 or 55.4%</b
					> of the gap. I.e., if ABHLO female employees earned the same
					as white male employees, the combined pay gap would shrink by
					55.4%.
				</p>
			</div>
		</section>
	</div>
</Scroller>

<Divider />

<Section>
	<h2>A Pathway Towards Closing Pay Gaps</h2>

	<blockquote class="text-indent" style="margin-top:50px">
		"Diversifying a few significant, well-paying career tracks can markedly
		reduce the City's pay gaps."
	</blockquote>
</Section>

<Section>
	<p>
		The City’s gender and racial pay gaps are not due to a few problematic
		agencies or titles, rather they are the result of occupational
		segregation throughout the City's workforce and pay disparities across
		occupations. These issues fall particularly hard on ABHLO female
		employees.
	</p>
	<p>
		To help narrow the City's pay gaps, the Data Team found that one
		effective strategy would be to diversify well-paid career tracks that
		are currently predominantly held by male and white employees. Below, we
		simulate the impact of this approach using a short list of titles that
		have a "high return" to this intervention, in terms of reducing pay
		gaps. Our results demonstrate that diversifying a few significant,
		well-paying career tracks can markedly reduce the City's pay gaps. The
		titles used in our simulations are just examples&mdash;diversification
		of similar titles beyond those listed would lead to further reduction of
		the pay gaps.
	</p>
</Section>

<!-- <Scroller threshold="1" splitscreen={true}>
	<div slot="background">
		<figure>
			<div class="scroll_tableLong">
				<Table
					tableData={$lowNonWhiteData}
					title="Populous Titles Held by Mostly Non-White Workers"
				/>
			</div>
		</figure>
	</div>

	<div slot="foreground">
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					The 17 jobs in this table employ 28,400 workers and employ
					mostly non-white workers. On average, their salaries are
					$28,700 below the City’s median pay ($76k).
				</p>
			</div>
		</section>
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					If we raised salaries for these titles to the City median,
					what would happen to the racial pay gap?
				</p>
			</div>
		</section>
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					We find that this scenario would <b
						>reduce the racial pay gap by 33%</b
					>. This would cost $1.3B in additional total labor cost,
					equivalent to 1.2% of the City’s $107B budget.
				</p>
			</div>
		</section>
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					Alternatively, <b
						>each $1,000 salary increase for these titles would
						reduce the racial pay gap by 1.16%</b
					> and it would cost $45.8M in additional total labor cost.
				</p>
			</div>
		</section>
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					Furthermore, as you can see in the table, these 17
					low-paying titles are also female-dominated. Therefore,
					raising salaries for these titles to the City median would
					not only reduce the racial pay gap, it would also <b
						>reduce the gender pay gap by 45% (or 1.58% for every
						$1,000 salary increase)</b
					>.
				</p>
			</div>
		</section>
	</div>
</Scroller>

<Divider /> -->

<Scroller threshold="1" splitscreen={true}>
	<div slot="background">
		<figure>
			<div class="scroll_tableShort">
				<Table
					tableData={$midMaleData}
					title="Populous Titles Predominantly Held by Male Employees"
				/>
			</div>
		</figure>
	</div>

	<div slot="foreground">
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					The nine titles in this table pay well and are predominantly
					held by male employees. In total, they employ 49,000
					workers, only 19% of whom are female. In contrast, NYC's
					overall workforce is 52% female.
				</p>
			</div>
		</section>
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					What would happen to the gender pay gap if the gender
					composition of these titles matched that of NYC's workforce?
					We find that this scenario would lead to a modest <b
						>reduction in the gender pay gap of 4.3%</b
					>.
				</p>
			</div>
		</section>
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					However, if both genders remain in these roles and fair
					promotion practices are used, greater diversity in these
					"entry-level" jobs would be expected to percolate up to
					higher-level positions over time.
				</p>
			</div>
		</section>
	</div>
</Scroller>

<Scroller threshold="1" splitscreen={true}>
	<div slot="background">
		<figure>
			<div class="scroll_tableShort" style="font-size: 0.7em">
				<Table
					tableData={$highMaleData}
					title="Advanced, Populous Titles Predominantly Held by Male Employees"
				/>
			</div>
		</figure>
	</div>

	<div slot="foreground">
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					The nine titles in this table are higher up the career
					ladder than those listed in the previous table. These
					high-level titles pay well, employ 14,000 workers, and are
					currently 86.5% male.
				</p>
			</div>
		</section>
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					What would happen to the gender pay gap if the gender
					composition of all 18 of these titles matched that of NYC's
					workforce? We find that this scenario would <b
						>reduce the gender pay gap by 37%</b
					>.
				</p>
			</div>
		</section>
		<section>
			<div class="col-medium">
				<p style="font-family:Georgia">
					Alternatively, for every <b
						>one percentage point increase in female representation
						in these roles, the gender pay gap would decrease by
						0.98%</b
					>.
				</p>
			</div>
		</section>
	</div>
</Scroller>

<Divider />

<Section>
	<p>
		We can apply the same approach to race/ethnicity. The two tables below
		list nine "entry-level" and nine "high-level" titles that are all
		predominantly held by white employees. These titles largely overlap with
		the 18 titles listed above. The nine "entry-level" jobs employ 44,000
		workers, 55% of whom are white. The nine "higher-level" titles employ
		14,000 workers and are over 60% white. In contrast, the racial
		composition of NYC's overall workforce is 68% ABHLO and 32% white.
	</p>
	<p>
		If the racial composition of these 18 titles matched that of NYC's
		workforce, what would happen to the racial pay gap? We find that such a
		scenario would lead to a <b>reduction in the racial pay gap of 24%</b>.
		Alternatively, for every
		<b
			>one percentage point increase in ABHLO representation in these
			roles, the racial pay gap would decrease by 0.84%</b
		>.
	</p>
</Section>

<Media col="medium" caption="">
	<div class="chart-sml">
		<Table
			tableData={$midWhiteData}
			title="Populous Titles Predominantly Held by White Employees"
		/>
	</div>
</Media>

<Media col="medium" caption="">
	<div class="chart-sml">
		<Table
			tableData={$highWhiteData}
			title="Advanced, Populous Titles Predominantly Held by White Employees"
		/>
	</div>
</Media>

<Divider />

<Section>
	<p>
		Admittedly, diversifying the career tracks mentioned is easier said than
		done. While steps have already been taken to improve diversity, cultural
		and workplace barriers prevent more female and ABHLO employees from
		joining and continuing in these careers. With this in mind, the Council
		proposes distributing workplace culture surveys to gain a deeper
		understanding of why occupational segregation exists and how to best
		address it.
	</p>
	<p>
		In addition to diversifying well-paying careers, the City should support
		municipal workers in lower-paying roles, which are currently
		predominantly held by female and ABHLO employees. To this end, the
		Council proposes offering career counseling services which would connect
		employees with professional growth and promotional opportunities,
		helping employees to move up their current career ladder or to move to
		better-paying municipal careers. Additionally, the Council looks forward
		to the results of the City's forthcoming comparable worth analysis,
		which will assess whether municipal titles are being paid equitably and
		could potentially help narrow the City's pay gaps.
	</p>
	<p>
		For further analysis, read the Data Team's full report <a
			style="color:inherit"
			href="http://council.nyc.gov/data/wp-content/uploads/sites/73/2024/04/PayDisparitiesReport2024.pdf"
			target="_blank">here</a
		>.
	</p>
</Section>

<DataTeamFooter />

<style>
	:global(svelte-scroller-foreground) {
		pointer-events: none !important;
	}
	:global(svelte-scroller-foreground section div) {
		pointer-events: all !important;
	}
	.scroll_tableLong {
		margin-top: 5vh;
		width: calc(75%);
	}
	.scroll_tableShort {
		height: 100%;
		width: 100%;
		padding-top: 15%;
		padding-left: 5%;
		padding-right: 5%;
		font-size: 0.8em;
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
