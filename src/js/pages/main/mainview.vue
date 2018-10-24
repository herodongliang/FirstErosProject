<template>
	<div>
		<wxc-tab-bar :tab-titles="tabTitles"
	               :tab-styles="tabStyles"
	               title-type="icon"
	               @wxcTabBarCurrentTabSelected="wxcTabBarCurrentTabSelected">
	    <!-- 第一个页面内容-->
	    <div class="item-container" :style="contentStyle"><main></main></div>

	    <!-- 第二个页面内容-->
	    <div class="item-container" :style="contentStyle"><second></second></div>

	    <!-- 第三个页面内容-->
	    <div class="item-container" :style="contentStyle"><third></third></div>

	    <!-- 第四个页面内容-->
	    <div class="item-container" :style="contentStyle"><mine></mine></div>
	  </wxc-tab-bar>

	<wxc-mask height="800"
	              width="702"
	              border-radius="0"
	              duration="200"
	              mask-bg-color="#FFFFFF"
	              :has-animation="true"
	              :has-overlay="true"
	              :show-close="true"
	              :show="show"
	              @wxcMaskSetHidden="wxcMaskSetHidden">
	      <div class="content">
	        <div class="demo-title">
	          <text class="title">Weex帮助你构建原生应用</text>
	        </div>
	        <text class="content-text">
	          与 Web App、HTML5 App 或 hybrid App 不同，您可以使用 Weex 构建一个真正的原生应用。更贴心的是你的代码只需使用 HTML、CSS、JavaScript
	          可以构建原生应用，上手非常简单。但实际上，应用的底层是 Objective-C 或 Java， 同时，Weex 提供很多 native 组件或模块供开发人员使用。
	        </text>
	      </div>
	    </wxc-mask>
	    <wxc-loading :show="loadingshow" type="default" need-mask='true' loading-text='加载中...'></wxc-loading>
  </div>
</template>
<script >
	import { WxcTabBar, Utils,WxcMask,WxcLoading } from 'weex-ui';
	import main from './main';
	import second from './second';
	import third from './third';
	import mine from './mine';
	export default {
    components: { WxcTabBar,WxcMask,WxcLoading,main,second,third,mine},
    data: () => ({
    	show:true,
    	loadingshow:false,
    	statusBarHeight:weex.config.eros.statusBarHeight,
      tabTitles: [
		    {
		      title: '社区',
		      icon: 'https://gw.alicdn.com/tfs/TB1MWXdSpXXXXcmXXXXXXXXXXXX-72-72.png',
		      activeIcon: 'https://gw.alicdn.com/tfs/TB1kCk2SXXXXXXFXFXXXXXXXXXX-72-72.png'
		    },
		    {
		      title: '咨询',
		      icon: 'https://gw.alicdn.com/tfs/TB1ARoKSXXXXXc9XVXXXXXXXXXX-72-72.png',
		      activeIcon: 'https://gw.alicdn.com/tfs/TB19Z72SXXXXXamXFXXXXXXXXXX-72-72.png'
		    },
		    {
		      title: '商城',
		      icon: 'https://gw.alicdn.com/tfs/TB1VKMISXXXXXbyaXXXXXXXXXXX-72-72.png',
		      activeIcon: 'https://gw.alicdn.com/tfs/TB1aTgZSXXXXXazXFXXXXXXXXXX-72-72.png',
		      badge: 5
		    },
		    {
		      title: '我的',
		      icon: 'https://gw.alicdn.com/tfs/TB1Do3tSXXXXXXMaFXXXXXXXXXX-72-72.png',
		      activeIcon: 'https://gw.alicdn.com/tfs/TB1LiNhSpXXXXaWXXXXXXXXXXXX-72-72.png',
		      dot: true
		    }
		  ],
        tabStyles: {
		    bgColor: '#FFFFFF',
		    titleColor: '#666666',
		    activeTitleColor: '#3D3D3D',
		    activeBgColor: '#FFFFFF',
		    isActiveTitleBold: true,
		    iconWidth: 50,
		    iconHeight: 50,
		    width: 140,
		    height: 100,
		    fontSize: 24,
		    textPaddingLeft: 10,
		    textPaddingRight: 10,
		    
		  },
    }),
    
    created () {
      const tabPageHeight = Utils.env.getPageHeight();
      // 如果页面没有导航栏，可以用下面这个计算高度的方法
       //const tabPageHeight = env.deviceHeight / env.deviceWidth * 750;
      const { tabStyles } = this;
      const totalHeight = weex.config.eros.statusBarHeight+tabPageHeight;
      this.contentStyle = { height: (tabPageHeight+44) + 'px' };//-tabStyles.height
      this.$notice.toast({
      	message:height
      })
    },
    eros:{
    	beforeAppear(){
    		this.loadingshow=true
    		this.$storage.get('main').then(resData => {
                    if(resData==='true'){
                        this.show=false
                    }
                })
    	},
    	appeared(){
    		this.loadingshow=false
    		this.$storage.set('main', 'true')
                        .then(resData => {}, error => {})
    	}
    },
    methods: {
      wxcTabBarCurrentTabSelected (e) {
        const index = e.page;
        this.$notice.toast({
        	message:e
        })

        // console.log(index);
      },
      wxcMaskSetHidden(){
      	this.show=false;
      	
      }
    }
  };
</script>
<style >
	.item-container {
    width: 750px;
    background-color: #f2f3f4;
    align-items: center;
    justify-content: center;
   
    
  }
  
</style>