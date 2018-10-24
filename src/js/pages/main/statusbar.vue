<template>
	<div class="status-bar" :style="{'height': statusBarHeight}"></div>
</template>
<script>
	export default{
		data(){
			return{
				statusBarHeight: weex.config.eros.statusBarHeight ? weex.config.eros.statusBarHeight : 40,
			}
		}
	}
</script>
<style>
	.status-bar {
	  width: 750;
	  height: 40;
	  background-color: #009ff0;
	}
</style>