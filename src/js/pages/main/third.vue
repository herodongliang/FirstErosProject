<template>
	<div class="wxc-demo" :style="{'padding-top':statusBarHeight}">
	<scroller class="scroller">

        <wxc-refresher ref="wxc-refresher"
                     scroller-ref="scroller"
                     main-text="下拉即可刷新"
                     pulling-text="释放即可刷新"
                     refreshing-text="加载中"
                     :max-time="5000"
                     :text-width="240"
                     @wxcRefresh="onRefresh"
                     @wxcTimeout="onTimeout"></wxc-refresher>

        <wxc-ep-slider :slider-id="autoSliderId"
                       :card-length='cardLength'
                       ref="wxc-ep-slider"
                       :card-s="cardSize"
                       :auto-play="false"
                       :select-index="4"
                       @wxcEpSliderPullMore="pullMore"
                       @wxcEpSliderCurrentIndexSelected="wxcEpSliderCurrentIndexSelected">
          <!--自动生成demo-->
          <wxc-pan-item v-for="(v,index) in itemlistrow"
                        :key="index"
                        :ext-id="index"
                        :class="['slider',`slider${index}`]"
                        @wxcPanItemPan="wxcPanItemPan"
                        @wxcPanItemClicked="wxcPanItemClicked(index)"
                        :slot="`card${index}_${autoSliderId}`"
                        :accessible="true"
                        :aria-label="`这里是第${index + 1}个滑块`">
                        <!-- 这里是第{{index + 1}}个滑块 -->
            <image class="text" src='bmlocal://assets/demo.jpg'></image>
          </wxc-pan-item>
          <!-- <div class="more-slider" slot="pull-more" :style="{left: `${cardLength * (  cardSize.width + cardSize.spacing)+60}px`,marginLeft:`${(750 - cardSize.width) / 2}px`}">
            <text class="textmore">加载更多</text>
          </div> -->
        </wxc-ep-slider>
        <div class="btn" @click="onClick">
          <text class="btn-text">手动切换2滑块</text>
        </div>

      <div class="demo">
        <wxc-noticebar notice="不配置参数的通告栏"></wxc-noticebar>
      </div>
      <div class="demo">
        <wxc-noticebar mode="link"
                       :notice-url="url"
                       notice="测试通告,可跳转"></wxc-noticebar>
      </div>
      <div class="demo">
        <wxc-noticebar :show="true"
                       mode="closable"
                       notice="测试通告，可关闭"></wxc-noticebar>
      </div>
      <div class="demo">
        <wxc-noticebar type="time"
                       notice="请在14分25秒内完成支付，超时将取消订单"></wxc-noticebar>
      </div>
      <div class="demo">
        <wxc-noticebar type="redbag"
                       notice="亲，你有很多很多红包可以用"></wxc-noticebar>
      </div>
      <div class="demo">
        <wxc-noticebar mode="link"
                       notice="一条测试通告,可跳转"
                       type="info"
                       @wxcNoticebarLinkClicked="wxcNoticebarLinkClicked"
                       :notice-url="url"></wxc-noticebar>
      </div>
      <div class="demo">
        <wxc-noticebar mode="closable"
                       notice="一条测试通告,关闭"
                       @wxcNoticebarCloseClicked="wxcNoticebarCloseClicked"
                       type="success"></wxc-noticebar>
      </div>
      <div class="demo">
        <wxc-noticebar mode="link"
                       notice="一行过长自动...的测试通告,可跳转测试通告,可跳转测试通告,可跳转可跳转测试通告,可跳转"
                       type="question"
                       :notice-url="url"></wxc-noticebar>
      </div>
      <div class="demo last-demo">
        <wxc-noticebar mode="closable"
                       notice="测试通告,可跳转测试通告,可跳转测试通告,可跳转测试通告,可跳转测试通告,可跳转"
                       :lines="2"
                       type="warn"></wxc-noticebar>
      </div>
    </scroller>

      </div>
</template>
<script>
	 import {WxcRefresher, WxcEpSlider, WxcPanItem, BindEnv,WxcNoticebar } from 'weex-ui';
	 const modal = weex.requireModule('modal');
	 export default{
	 	components:{WxcRefresher,WxcEpSlider, WxcPanItem,WxcNoticebar},
	 	data:()=>({
	 		autoSliderId: 2,
	 		cardLength: 5,
	 		statusBarHeight:weex.config.eros.statusBarHeight,
	 		cardSize: {
		        width: 400,
		        height: 300,
		        spacing: 0,
		        scale: 0.8
		      },
        itemlistrow:[],
		    url: 'https://h5.m.taobao.com/trip/weex-ui/index.html?_wx_tpl=https%3A%2F%2Fh5.m.taobao.com%2Ftrip%2Fweex-ui%2Fdemo%2Findex.native-min.js',
		    
	 	}),
	 	created(){
      for(let i=0;i<5;i++){
        this.itemlistrow.push(i)
      }
	 		// if (weex && weex.config && weex.config.eros) {
	 		// 	this.$notice.toast({
	 		// 		message:weex.config.eros['statusBarHeight']
	 		// 	})
	 		// }
	 	},
	 	methods:{
      onTimeout () {
        this.sto && clearTimeout(this.sto);
        modal.toast({ message: '刷新超时，可定义超时时间', duration: 1 });
      },
      onRefresh (e) {
        this.sto = setTimeout(()=>{ this.refreshSucc() }, this.refreshTime)
        modal.toast({ message: '刷新中...', duration: .5 });
        this.refreshTime = this.refreshTime === 3000 ? 10000 : 3000;
      },
      refreshSucc () {
        this.$refs['wxc-refresher'].wxcCancel();
        modal.toast({ message: '刷新成功', duration: .5 });
      },
      refreshclick(){
        this.$router.open({
          name:'result'
        })
      },

    
      pullMore(){
        this.$notice.toast({
          message:'加载更多'
        });
        setTimeout(() => {
          this.$notice.toast({
            message:'加载完成'
          })
          for(let i=5;i<9;i++){
                  this.itemlistrow.push(i)
                }
        }, 2000)
      },
	 	  wxcEpSliderCurrentIndexSelected (e) {
	       const index = e.currentIndex;
	       console.log(index);
         // this.$notice.toast({
         //    message:index
         // })
	      },
	 		onClick () {
	        // 手动切换到第2张
	        this.$refs['wxc-ep-slider'].manualSetPage(1);
	      },
	       wxcPanItemPan (e) {
	        if (BindEnv.supportsEBForAndroid()) {
	          this.$refs['wxc-ep-slider'].clearAutoPlay()
	          this.$refs['wxc-ep-slider'].bindExp(e.element)
	          this.$nitice.toast({
	          	message:e
	          })
	        }
	      },
	      wxcPanItemClicked (num) {
	        this.$notice.toast({
	          message: num,
	        })
	        this.$router.open({
	        	name:'refresh'
	        })
      	  },
		 wxcNoticebarCloseClicked (e) {
		        console.log(e);
		        modal.toast({ 'message': 'wxcNoticebarCloseClicked', 'duration': 1 });
		      },
		  wxcNoticebarLinkClicked (e) {
		        console.log(e);
		        modal.toast({ 'message': 'wxcNoticebarLinkClicked', 'duration': 1 });
		      }
	 	}
	 }
</script>
<style lang="sass" scoped >
	@import 'src/js/css/base';
	.wxc-demo {
	padding-top:44;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    
  }
  .textmore{
    justify-content:center;
    align-items:center;
  }
  .more-slider{
    width: 100px;
    height: 300px;
    background-color: #ffc302;
  }
  .scroller {
    flex: 1;
  }
  .demo {
    margin-top: 40px;
  }
  .last-demo {
    margin-bottom: 40px;
  }
  .text {
  	width: 180;
  	height: 180;
    
  }
    .btn {
    width: 250px;
    height: 80px;
    margin-left: 250px;
    margin-bottom: 40px;
    margin-top: 40px;
    background-color: #635147;
    align-items: center;
    justify-content: center;
  }
  .btn-text {
    font-size: 30px;
    color: #ffffff;
  }
  .slider {
    width: 400px;
    height: 300px;
    background-color: #c3413d;
    align-items: center;
    justify-content: center;
  }
  .slider1 {
    background-color: #635147;
  }
  .slider2 {
    background-color: #ffc302;
  }
  .slider3 {
    background-color: #ff9090;
  }
  .slider4 {
    background-color: #546e7a;
  }
  .slider5 {
    background-color: #635147;
  }
  .slider6 {
    background-color: #ffc302;
  }
  .slider7 {
    background-color: #ff9090;
  }
  .slider8 {
    background-color: #546e7a;
  }
</style>