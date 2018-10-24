<template>
  <scroller class="scroller">
    <refresh class="loading" @refresh="onrefresh" :display="refreshing ? 'show' : 'hide'">
      <text class="indicator-text">refreshing ...</text>
      <loading-indicator class="indicator"></loading-indicator>
    </refresh>
    <div class="cell" v-for="num in lists">
      <div class="panel">
        <text class="text">{{num}}</text>
      </div>
    </div>
    <loading class="loading" @loading="onloading" :display="loadinging ? 'show' : 'hide'">
      <!-- <loading-indicator class="indicator"></loading-indicator> -->
    </loading>
  </scroller>
</template>

<script>
  const modal = weex.requireModule('modal')

  export default {
    data () {
      return {
        loadinging: false,
        refreshing:false,
        lists: [1, 2, 3, 4, 5]
      }
    },
    methods: {
      onloading (event) {
        modal.toast({ message: 'Loading', duration: 1 })
        this.loadinging = true
        setTimeout(() => {
          this.loadinging = false
          for(let i=6;i<10;i++){
            this.lists.push(i)
          }
        }, 2000)
      },
      onrefresh (event) {
        modal.toast({ message: 'refreshing', duration: 1 })
        this.refreshing = true
        setTimeout(() => {
          this.refreshing = false
        }, 1000)
      },
    }
  }
</script>

<style scoped>
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
  .indicator-text {
    color: #888888;
    font-size: 42px;
    text-align: center;
  }
  .indicator {
    margin-top: 10px;
    height: 40px;
    width: 40px;
    color: blue;
  }
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
</style>