<template>
	<div id="app">
		<router-view v-if="isRouterAlive"></router-view>
	</div>
</template>

<script>
	import Qs from 'qs'
	import axios from 'axios'
	export default {
		name: 'App',
		provide(){
			return {
				reload:this.reload
			}
		},
		data(){
			return{
				isRouterAlive:true
			}
		},
		methods: {
			reload(){
				this.isRouterAlive=false
				this.$nextTick(function(){
					this.isRouterAlive=true
				})
			}
		},
		mounted:function(){
			document.title=this.$route.meta.title;
			/*if(!localStorage.getItem('openid')){
				window.location.href = 'http://cloud.sokeed.com/mp/card/error.html'
			}*/
		}
	}
</script>
<style>
	#app {
		background: #efefef;
	}
</style>