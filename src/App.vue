<template>
	<div class="bg">
		<CalculatorComponent/>
	</div>
</template>

<script>
	import CalculatorComponent from './components/CalculatorComponent.vue'
	import './assets/tailwind.css'

	export default {
		components: {
			CalculatorComponent
		}
	}
</script>

<style>
	.bg {
		background-color: #F5F5F5;
		width: 100vw;
		height: 100vh;
		padding: 2rem;
	}
	html, body {
		font-family: 'Sora', sans-serif;
	}
</style>
