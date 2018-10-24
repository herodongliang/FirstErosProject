<template>
  <div class="wxc-demo" :style="{'padding-top':paddingheight}">
    <list class="scroller" ref="scroller">
      <wxc-refresher ref="wxc-refresher"
                     scroller-ref="scroller"
                     main-text="下拉即可刷新"
                     pulling-text="释放即可刷新"
                     refreshing-text="加载中"
                     :max-time="5000"
                     :text-width="240"
                     @wxcRefresh="onRefresh"
                     @wxcTimeout="onTimeout"></wxc-refresher>
      <cell class="cell" v-for="(num,key) in lists" :key="key">
        <!-- <div class="panel" @click="refreshclick">
          <text class="text">{{num}}</text>
        </div> -->
         <wxc-ep-slider :slider-id="key"
                       :card-length='cardLength'
                       ref="wxc-ep-slider"
                       :card-s="cardSize"
                       :auto-play="false"
                       :select-index="2"
                       @wxcEpSliderCurrentIndexSelected="wxcEpSliderCurrentIndexSelected">
          
        <wxc-pan-item v-for="(v,index) in [1,2,3,4,5]"
                      :key="index"
                      :ext-id="index"
                      :class="['slider',`slider${index}`]"
                      @wxcPanItemPan="wxcPanItemPan"
                      @wxcPanItemClicked="wxcPanItemClicked(index)"
                      :slot="`card${index}_${key}`"
                      :accessible="true"
                      :aria-label="`这里是第${index + 1}个滑块`">
                      
          <image class="image" src='bmlocal://assets/demo.jpg'></image>
        </wxc-pan-item>
        </wxc-ep-slider>
      </cell>
    </list>
  </div>
</template>

<script>
  import { WxcRefresher,WxcEpSlider, WxcPanItem, } from 'weex-ui';
  const modal = weex.requireModule('modal');
  export default {
    components: { WxcRefresher ,WxcEpSlider, WxcPanItem,},
    data: () => ({
      lists: ['下拉刷新', 'Drop Down', '↓', '↓', '↓', '↓', '↓', '↓'],
      paddingheight:weex.config.eros.statusBarHeight,
      refreshTime: 3000,
      autoSliderId: 2,
      cardLength: 5,
      cardSize: {
            width: 400,
            height: 300,
            spacing: 0,
            scale: 0.8
          },
    }),
   
    methods: {
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
    }
  };
</script>

<style lang="sass" scoped>
@import 'src/js/css/base';
  
  .panel {
    width: 600px;
    height: 250px;
    margin-left: 75px;
    margin-top: 35px;
    margin-bottom: 35px;
    flex-direction: column;
    justify-content: center;
    border-width: 2px;
    border-style: solid;
    border-color: #DDDDDD;
    background-color: #F5F5F5;
  }
  .text {
    font-size: 50px;
    text-align: center;
    color: #41B883;
  }
  .scroller{
    flex: 1;
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
  .image {
    width: 180;
    height: 180;
    
  }
</style>