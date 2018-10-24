<template>
    <div style="margin-top: 50px;">
        <text class="title">Hello，</text>
        <text class="title">developer</text>
        <wxc-button class="btn-250" text="show eros" @wxcButtonClicked="showEros"></wxc-button>
        <div class="loginorregister">
            <wxc-button class="btn-250" text="注册" @wxcButtonClicked="jump('register')"></wxc-button>
            <wxc-button class="btn-250" text="登陆" @wxcButtonClicked="jump('mainview')"></wxc-button>
        </div>
        <div class="imagelayout">
            <image class="image" src='bmlocal://assets/demo.jpg' @click="imageclick"></image>
            <text class="text">&#xe606;&#xe605;</text>
        </div>
        <wxc-lightbox
          ref="wxc-lightbox"
          height="800"
          :show="show"
          :image-list="imageList"
          @wxcLightboxOverlayClicked="wxcLightboxOverlayClicked">
        </wxc-lightbox>

    </div>
</template>
<script>
    import { WxcButton ,WxcLightbox} from 'weex-ui';
    var modal = weex.requireModule('modal');
    export default {
        components: { WxcButton ,WxcLightbox},
        beforeCreate(){
            domModule.addRule('fontFace',{
                'fontFamily':"iconfont",
                'src':"url('bmlocal://iconfont/font_1469606063_76593.ttf')"
            })
        },
        data(){
            return{
                curHomeBackTriggerTimes: 1,
                maxHomeBackTriggerTimes: 2,
                show:false,
                imageList:[
                {src:"http://p0.so.qhimgs1.com/bdr/200_200_/t010c4669fd5734bb30.jpg"},
                {src:"http://p0.so.qhmsg.com/t011eb34023c3dd26c5.jpg"},
                {src:"http://img15.3lian.com/2015/a1/34/d/167.jpg"}
                ]
            };
        },
        created(){
            // 安卓自定义退出 app
            this.androidFinishApp()
        },
        eros:{
            beforeBackAppear(params){
                //console.log(params),
                modal.toast({message:'hello'+params})
            },
             beforeAppear (params, options) {
                console.log('beforeAppear');
                this.$storage.get('name').then(resData => {
                    console.log(resData) // weex-eros
                    this.$notice.toast({
                        message:resData
                    })
                    // if(resData==='haslogin'){
                    //     this.$router.open({
                    //         name:'main'
                    //     })
                    // }
                    this.$event.on('eventName',(params) => {
                        // params 为触发该事件所传的参数
                        this.$notice.toast({
                            message:params
                        })
                    });
                })
            },
        },
        methods: {
            wxcLightboxOverlayClicked(){
                this.show=false
            },
            // 这里给按钮添加 showEros 事件来跳转
            showEros() {
                this.$router.open({
                    name: 'eros',
                    type:'PRESENT',
                    backCallback: () => {
                        this.$notice.toast({
                            message: '页面返回时的回调触发了，但是返回事件被覆盖了，我们需要重写'
                        })
                        this.$router.back({
                            type: 'PRESENT'
                        })
                    }
                })
            },
            androidFinishApp () {
                const globalEvent = weex.requireModule('globalEvent')
                globalEvent.addEventListener('homeBack', options => {
                    (this.curHomeBackTriggerTimes === this.maxHomeBackTriggerTimes) && this.$router.finish()
                    this.curHomeBackTriggerTimes++
                    this.$notice.toast({ message: `点击返回${this.maxHomeBackTriggerTimes}次之后，会关闭应用，当前点击第${this.curHomeBackTriggerTimes}次` })
                })
            },
            jump(name){
                this.$router.open({
                    name:name
                })
                
                if(name==='register'){
                    
                    this.$storage.delete('main').then(resData=>{
                        this.$notice.toast({
                            message:'删除完成'
                        })
                    })
                }
            },
            imageclick(){
                this.show=true
                // this.$image.preview({
                //     index: 0,                     // 所点击图片下标
                //     images: ['http://p0.so.qhimgs1.com/bdr/200_200_/t010c4669fd5734bb30.jpg',
                //     'http://p0.so.qhmsg.com/t011eb34023c3dd26c5.jpg'
                //     ,'http://img15.3lian.com/2015/a1/34/d/167.jpg'],        // 图片的网络地址
                // })
            }
        },

    }
</script>
<style>
.text{
    font-family: iconfont;
}
.title{
    font-size: 60;
}
.btn-250{
    margin-left: 20;
    width: 250;
}
.imagelayout{
    align-items: center;
}
.image{
    margin-top:20;
    width: 500;
    height: 500;
    
}
.loginorregister{
    justify-content: center;
    align-items: center;
    margin-top: 20;
    margin-left: 20;
    flex-direction:row;
}
</style>