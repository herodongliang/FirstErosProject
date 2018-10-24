<template>
	<div class="layout">
		<div class="phoneandcode">
			<input class="input phonenumber" placeholder="请输入手机号"></input>
			<text class="codetext">验证码</text>
		</div>
		<input class="input password" placeholder="请输入密码"></input>
		<input class="input password" placeholder="请确认密码"></input>
		<wxc-button class="button" text="确定" @wxcButtonClicked="wxcButtonClicked"></wxc-button>
		<div>
			<list ref='ref' :showRefresh='true' @refresh="onRefresh">

				<cell v-for="(num,index) in list" :key="index" :index="index">
					<!-- <wxc-cell :title="这是第${num}条数据"></wxc-cell> -->
					 <wxc-cell :title="`这是第${num}条数据`"
                    :has-arrow="false"
                    @wxcCellClicked="wxcCellClicked"
                    :has-top-border="true"></wxc-cell>
				</cell>
			</list>
		</div>
	</div>
</template>
<script >
	import {WxcButton,WxcCell} from 'weex-ui';
	var modal = weex.requireModule('modal');
	export default{
		components :{WxcButton,WxcCell},
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
		methods:{
			onRefresh(){
				setTimeout(()=>{
					this.$refs['ref'].refreshEnd()
					this.$notice.toast({
						message:'刷新完成'
					})
				},2000)
			},
			wxcButtonClicked(){
				this.$router.back({
					length:1,
					type:'PUSH',
					callback(){
						modal.toast({message:"我是注册页面"})
					}
				}),
				this.$router.serBackParams({
					name:"董亮"
				})
			}
		}
	}
</script>
<style >
	@import 'src/js/css/base';
	.layout{
		background-color: #F0F0F0;
	}
	.phoneandcode{
		width: 700;
		/*margin-left: 20;*/
		flex-direction: row;
		/*justify-content: center;*/
	}
	.input{
		tint-color:#000000;
		padding-left: 10;
		margin-top:20;
		font-size: 32;
		color:#000000;
		margin-left: 20;
		height: 80;
		border-color: #C0C0C0;
		border-width: 2;
		line-height: 80;
		border-radius: 15;
	}
	.phonenumber{
		type:number;
		width: 500;
	}
	.password{
		width: 700;
		type:password;
	}
	.codetext{
		text-align: center;
		line-height: 80;
		width: 160;
		height: 80;
		margin-top: 20;
		margin-left: 20;
		border-width: 2;
		background-color: #FF0000;
		border-color: #FF0000;
		border-radius: 15;
	}
	.button{
		margin-top:20;
		margin-left: 20;
	}
</style>