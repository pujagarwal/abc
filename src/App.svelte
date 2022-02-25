<script>
	import { onMount } from "svelte";

	export const myName = "Puja Agarwal";

	let data;
    let xScaleNew;
    let xScaleTicks = [];
     
	onMount(async () => {
		const fetched = await fetch("./static/census2000.json");
		let rawData = (await fetched.json()).demographic_groups;
		console.log(rawData);

        console.log ("before filtered:", rawData.length);
		data = rawData.filter((record) => {
        return record.year == "2000" && record.sex == "F";
});
console.log ("after filtered:", data.length);

let arrayofOnlyPopulation = data.map((record) => {
    return record.people;
})
console.log(arrayofOnlyPopulation);
maximunPopulation = Math.max(...arrayofOnlyPopulation);
console.log(maximunPopulation);
});

let maximunPopulation = 9999999999;
let maximunWidth = 400;
function xScale(value){
return (value / maximunPopulation)
}
</script>

<main>
	<h1>In-Class Acitivity by {myName}!</h1>

	<svg id="visualization">
		{#if data !== undefined}
			{#each data as record, index}
				<g transform="translate(50, {index * 10 + 50})">
                <rect class="bar" x="0" y="0" width = {record.people / 50000} height = "8" />
                <text x="-40" y="10">{record.age}</text>
                xScale(record.people);
				</g>
			{/each}
		{/if}
	</svg>

</main>

<style>
	h1 {
		font-size: 1.5rem;
	}
	svg#visualization {
		width: 600px;
		height: 500px;
	}
	text {
		font-size: 0.8rem;
	}
  
    .bar {
        fill: steelblue;
}

</style>
