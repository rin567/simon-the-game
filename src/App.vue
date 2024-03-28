<template>
	<div id="app">
		<h1>Simon the game</h1>
		<Field />
	</div>
</template>

<script>
import Field from './components/Field.vue'
export default {
	name: 'App',
	components: {
		Field,
	},
}
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	background-color: #70a8ad;
	height: 100%;
	display: flex;
	flex-flow: column wrap;
	align-items: center;
	justify-content: flex-start;
}
</style>
