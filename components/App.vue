<template>
	<div class="Chart">
		<h2>Chart</h2>
		<LineChart />
	</div>
</template>

<script>
	import LineChart from './LineChart.js'

	export default {
		components: {
			LineChart
		}
	}
</script>

<style>
	.Chart h2 {
		margin-top: 0;
		padding: 15px 0;
		color:  rgba(255, 0,0, 0.5);
		border-bottom: 1px solid #323d54;
	}
</style>