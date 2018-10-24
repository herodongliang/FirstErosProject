<template>
	<div class="wxc-demo" :style="{'padding-top':statusBarHeight}">
	  <wxc-tab-page ref="wxc-tab-page"
	                :tab-titles="tabTitles"
	                :tab-styles="tabStyles"
	                title-type="icon"
	                :tab-page-height="tabPageHeight"
	                @wxcTabPageCurrentTabSelected="wxcTabPageCurrentTabSelected">
	    <list v-for="(v,index) in tabList"
	          :key="index"
	          class="item-container"
	          :style="{ height: (tabPageHeight - tabStyles.height) + 'px' }">
		      <cell class="border-cell"></cell>
		      <cell v-for="(demo,key) in v"
		            class="cell"
		            :key="key">
	        <wxc-pan-item :ext-id="'1-' + (v) + '-' + (key)"
	                      url="https://h5.m.taobao.com/trip/ticket/detail/index.html?scenicId=2675"
	                      @wxcPanItemPan="wxcPanItemPan">
	         <div class="content">
	            <text>{{key}}</text>
	         </div>
	        </wxc-pan-item>
	      </cell>
	    </list>
	  </wxc-tab-page>
  </div>
</template>

<style lang="sass" scoped>
  @import 'src/js/css/base';
  .item-container {
    width: 750px;
    background-color: #f2f3f4;
  }

  .border-cell {
    background-color: #f2f3f4;
    width: 750px;
    height: 24px;
    align-items: center;
    justify-content: center;
    border-bottom-width: 1px;
    border-style: solid;
    border-color: #e0e0e0;
  }

  .cell {
    background-color: #ffffff;
  }


  .content{
    width:750px;
    height:300px;
    border-bottom-width:1px;
    align-items: center;
    justify-content: center;
  }
</style>
<script>
  const dom = weex.requireModule('dom');
  import { WxcTabPage, WxcPanItem, Utils, BindEnv } from 'weex-ui';

  // https://github.com/alibaba/weex-ui/blob/master/example/tab-page/config.js
  

  export default {
    components: { WxcTabPage, WxcPanItem },
    data: () => ({
      statusBarHeight:weex.config.eros.statusBarHeight,
      tabTitles: [
    {
      title: '热门跟团',
      icon: 'https://gw.alicdn.com/tfs/TB1MWXdSpXXXXcmXXXXXXXXXXXX-72-72.png',
      activeIcon: 'https://gw.alicdn.com/tfs/TB1kCk2SXXXXXXFXFXXXXXXXXXX-72-72.png',
    },
    {
      title: '云南跟团',
      icon: 'https://gw.alicdn.com/tfs/TB1ARoKSXXXXXc9XVXXXXXXXXXX-72-72.png',
      activeIcon: 'https://gw.alicdn.com/tfs/TB19Z72SXXXXXamXFXXXXXXXXXX-72-72.png'
    },
    {
      title: '四川跟团',
      icon: 'https://gw.alicdn.com/tfs/TB1VKMISXXXXXbyaXXXXXXXXXXX-72-72.png',
      activeIcon: 'https://gw.alicdn.com/tfs/TB1aTgZSXXXXXazXFXXXXXXXXXX-72-72.png'
    },
    // {
    //   title: '海南跟团',
    //   icon: 'https://gw.alicdn.com/tfs/TB1Do3tSXXXXXXMaFXXXXXXXXXX-72-72.png',
    //   activeIcon: 'https://gw.alicdn.com/tfs/TB1LiNhSpXXXXaWXXXXXXXXXXXX-72-72.png'
    // },
    // {
    //   title: '北京跟团',
    //   icon: 'https://gw.alicdn.com/tfs/TB1jFsLSXXXXXX_aXXXXXXXXXXX-72-72.png',
    //   activeIcon: 'https://gw.alicdn.com/tfs/TB1_Kc.SXXXXXa8XpXXXXXXXXXX-72-72.png'
    // },
    // {
    //   title: '出境跟团',
    //   icon: 'https://gw.alicdn.com/tfs/TB199sPSXXXXXb4XVXXXXXXXXXX-72-72.png',
    //   activeIcon: 'https://gw.alicdn.com/tfs/TB1DR.3SXXXXXc2XpXXXXXXXXXX-72-72.png'
    // }, {
    //   title: '出境日游',
    //   icon: 'https://gw.alicdn.com/tfs/TB1hedfSpXXXXchXXXXXXXXXXXX-72-72.png',
    //   activeIcon: 'https://gw.alicdn.com/tfs/TB1mrXaSpXXXXaqXpXXXXXXXXXX-72-72.png'
    // },
    // {
    //   title: '邮轮',
    //   icon: 'https://gw.alicdn.com/tfs/TB1twhkSpXXXXXLXXXXXXXXXXXX-72-72.png',
    //   activeIcon: 'https://gw.alicdn.com/tfs/TB1dhlhSpXXXXa8XXXXXXXXXXXX-72-72.png'
    // }
  ],
  tabStyles: {
    bgColor: '#FFFFFF',
    titleColor: '#666666',
    activeTitleColor: '#3D3D3D',
    activeBgColor: '#FFFFFF',
    isActiveTitleBold: true,
    iconWidth: 50,
    iconHeight: 50,
    width: 120,
    height: 100,
    fontSize: 24,
    leftOffset:195,
    hasActiveBottom: true,
    activeBottomColor: '#FFC900',
    activeBottomHeight: 6,
    activeBottomWidth: 120,
    textPaddingLeft: 10,
    textPaddingRight: 10
  },
      tabList: [],
      demoList: [1, 2, 3, 4, 5, 6, 7, 8, 9],
      tabPageHeight: 1334
    }),
    created () {
      this.tabPageHeight = Utils.env.getPageHeight();
      this.tabList = [...Array(this.tabTitles.length).keys()].map(i => []);
      Vue.set(this.tabList, 0, this.demoList);
    },
    methods: {
      wxcTabPageCurrentTabSelected (e) {
        const self = this;
        const index = e.page;
        /* Unloaded tab analog data request */
        if (!Utils.isNonEmptyArray(self.tabList[index])) {
          setTimeout(() => {
            Vue.set(self.tabList, index, self.demoList);
          }, 100);
        }
      },
      wxcPanItemPan (e) {
        if (BindEnv.supportsEBForAndroid()) {
          this.$refs['wxc-tab-page'].bindExp(e.element);
        }
      },
      onRefresh(){
      	const self = this;
			setTimeout(()=>{
				self.$notice.toast({
					message:self.$refs
				})
				//self.$refs["tab-list"].refreshEnd()
				// self.$notice.toast({
				// 	message:'刷新完成'
				// })
			},500)
		},
    }
  };
</script>