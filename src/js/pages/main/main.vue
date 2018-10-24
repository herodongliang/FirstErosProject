<template>
	<div class="wxc-demo" :style="{'padding-top':statusbarheight}">
	 	<div class="demo">
        <wxc-noticebar type="redbag"
       					mode="closable"
                       notice="您的红包有效期仅剩1天"></wxc-noticebar>
		<list ref='ref' :showRefresh='true' @refresh="onRefresh" :style="{'padding-bottom':100}">
			
			<cell v-for="(num,index) in list" :key="index" :index="index">
				<wxc-cell :title="`这是第${num}条数据`"
					label="标题"
					desc="哈哈哈哈或"
                    :has-arrow="true"
                    @wxcCellClicked="wxcCellClicked"
                    :has-top-border="true">
                </wxc-cell>
			</cell>
			<loading class="loading" @loading="onloading" :display="loadinging ? 'show' : 'hide'">
     
    		</loading>
			
		</list>
		<wxc-popup popup-color="rgb(92, 184, 92)"
               :show="isBottomShow"
               @wxcPopupOverlayClicked="popupOverlayBottomClick"
               pos="bottom"
               height="800">
	      <div class="demo-content">
	      	<wxc-part-loading :show="partloadshow"></wxc-part-loading>
	      	<div :show="contentshow">
		        <image src="bmlocal://assets/demo.jpg" class="demo-image"></image>
		        <text>与 Web App、HTML5 App 或 hybrid App 不同，您可以使用 Weex 构建一个真正的原生应用。更贴心的是你的代码只需使用 HTML、CSS、JavaScript 可以构建原生应用，上手非常简单。
		        </text>
	        </div>
	      </div>
	    </wxc-popup>
	    </div>
	</div>
</template>
<script>
	import {WxcCell,WxcPopup,WxcLoading,WxcPartLoading,WxcNoticebar} from 'weex-ui';
	import Statusbar from './statusbar';
	export default{
		components:{WxcCell,Statusbar,WxcPopup,WxcLoading,WxcPartLoading,WxcNoticebar},
		data(){
			return{
				list:[],
				loadinging:false,
				loading:false,
				isBottomShow:false,
				partloadshow:false,
				contentshow:false,
				statusbarheight:weex.config.eros.statusBarHeight
			}
		},
		created(){
			for(let i=0;i<15;i++){
				this.list.push(i+1)
			}
		},

		methods:{
			onloading (event) {
			        this.$notice.toast({ 
			        	message: 'Loading'
			        })
			        this.loadinging = true
			        setTimeout(() => {
			          this.loadinging = false
			          for(let i=6;i<10;i++){
			            this.list.push(i)
			          }
			        }, 2000)
			      },
			
			popupOverlayBottomClick(){
				this.isBottomShow=false;
			},
			onRefresh(){
				
				setTimeout(()=>{
					for(let i=10;i<20;i++){
						this.list.push(i+1)
					}
					this.$refs['ref'].refreshEnd()
					this.loadingshow=false
					this.$notice.toast({
						message:'刷新完成'

					})
				},2000)
			},
			wxcCellClicked:function(data){
				this.isBottomShow=true
				this.partloadshow=true
				setTimeout(()=>{
					this.contentshow=true
					this.partloadshow=false
				},1000)
				
				// this.$router.open({
				// 	name:'nine'
				// })
				// this.$event.emit("eventname",this.inputtext);
				// this.$notice.toast({
				// 	message:this.inputtext
				// })
				// this.$router.back()
			},
		
		}
	}
</script>
<style lang="sass" scoped >
	@import 'src/js/css/base';
	
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
	.demo-image{
		width: 200;
		height: 200;
		align-items: center;
	}
.loading {
    width: 750;
    display: -ms-flex;
    display: -webkit-flex;
    display: flex;
    -ms-flex-align: center;
    -webkit-align-items: center;
    -webkit-box-align: center;
    align-items: center;
  }
	
</style>