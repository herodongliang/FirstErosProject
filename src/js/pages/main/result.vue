<template>
  <div class="wxc-demo">
    <wxc-result :type="type"
                padding-top="232"
                :custom-set="customSet"
                @wxcResultButtonClicked="resultButtonClick"
                :show="show"></wxc-result>
  </div>
</template>

<style scoped>
  .wxc-demo {
    position: absolute;
    top: 0;
    right: 0;
    left: 0;
    bottom: 0;
    background-color: #ffffff;
  }
</style>

<script>
  const modal = weex.requireModule('modal');
  import { WxcResult } from 'weex-ui';
  
  export default {
    components: { WxcResult },
    data: () => ({
      show: true,
      type: 'noNetwork',
      customSet:{
        errorPage: {
          button: '立即反馈',
          content: '亲，出错了',
          pic: 'https://gw.alicdn.com/tfs/TB1lgzNfBHH8KJjy0FbXXcqlpXa-320-320.png'
        }
      }
    }),
    computed: {},
    
    methods: {
      resultButtonClick (e) {
        modal.toast({
          'message': `你点击了按钮`,
          'duration': 1
        })
        this.$router.back()
      }
    }
  };
</script>