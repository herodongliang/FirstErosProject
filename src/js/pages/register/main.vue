<template>
	<div>
		<list ref='ref' :showRefresh='true' @refresh="onRefresh">
			<cell v-for="(num,index) in list" :key="index" :index="index">
				<wxc-cell :title="`这是第${num}条数据`"
                    :has-arrow="false"
                    @wxcCellClicked="wxcCellClicked"
                    :has-top-border="true"></wxc-cell>
			</cell>
		</list>
	</div>
</template>
<script>
	import {WxcCell} from 'weex-ui';
	export default{
		components:{WxcCell},
		data(){
			return{
				list:[],

			}
		},
		created(){
			for(let i=0;i<20;i++){
				this.list.push(i+1)
			}
		},
		methods(){
			onRefresh(){
				setTimeout(()=>{
					this.$refs['ref'].refreshEnd()
					this.$notice.toast({
						message:'刷新完成'
					})
				},2000)
			}
		}
	}
</script>
<style>
	
</style>