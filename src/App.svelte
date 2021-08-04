<script>
	import sped from './sped.json';
	import { onMount } from 'svelte';
	import * as d3 from 'd3';

	let selectedDistrict;
	let el;
	const data = [30, 86, 168, 281, 303, 365]

	const height = 100
	const width = 500

	onMount(() => {
		d3.select(el)
			.selectAll("div")
			.data(data)
			.enter()
			.append("div")
			.style("width", function(d) {
				return d + "px";
			})
			.text(function(d) {
				return d;
			});
	});
	
</script>

<main>
	{#if selectedDistrict}
		<!-- svelte-ignore a11y-invalid-attribute -->
		<p id="home" on:click={() => selectedDistrict = null}>Go back to district list</p>
		<h1>{selectedDistrict['name']}</h1>
		<p>The percent of students with IEPs who are in a regular classroom for at least 80% of the day is: {selectedDistrict['eighty']}</p>
		<svg id="eighty-bar" height=50 width=500>
			<rect y=0 x=0 height=50 width={selectedDistrict['eighty'] * 5} fill=lightblue></rect>
		</svg>
		<svg id="eighty-hist" height=100 width=500>
			<line x1=100 x2=100 y1=0 y2=100 stroke=#ff3e00 stroke-width="2"></line>
		</svg>
		<p>The percent of students with IEPs who are in a regular classroom for less than 40% of the day is: {selectedDistrict['forty']}</p>
		<svg id="forty-bar" height=50 width=500>
			<rect y=0 x=0 height=50 width={selectedDistrict['forty'] * 5} fill=lightblue></rect>
		</svg>
	{:else}
		<h3>Oregon school districts</h3>
		<p>Select a district to see its special education data</p>
		<ul>
			{#each sped as district}
				<li on:click={() => selectedDistrict = district}>{district['name']}</li>
			{/each}
		</ul>
	{/if}
</main>
<div class="chart" bind:this={el}></div>

<style>
	main {
		/* text-align: center; */
		padding: 1em;
		/* max-width: 240px; */
		margin: 0 auto;
	}
	
	svg {
		border: 1px solid lightblue;
		display: block;
	}

	li {
		cursor: pointer;
	}

	#home {
		cursor: pointer;
	}

	#eighty-hist {
		margin-top: 20px;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	ul {
		list-style-type: none;
	}

	.chart :global(div) {
		font: 10px sans-serif;
		background-color: lightblue;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>