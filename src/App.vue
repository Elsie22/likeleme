<template>
<div id="app">
  <ele-header :seller="seller"> </ele-header>
  <div class="tab">
    <div class="tabitem">
      <router-link to="/goods" >商品</router-link>
    </div>
    <div class="tabitem">
      <router-link to="/ratings">评价</router-link>
    </div>
    <div class="tabitem">
      <router-link to="/seller">商家</router-link>
    </div>
  </div>
  <keep-alive>
    <router-view :seller="seller"></router-view>
  </keep-alive>
</div>
</template>

<script>
import axios from 'axios'
import header from '@/components/header/Header'
const OK = 0
export default {
  name: 'App',
  data () {
    return {
      seller: {}
    }
  },
  created () {
    this.getSeller()
  },
  methods: {
    getSeller () {
      axios.get('/api/seller')
        .then(res => {
          if (res.data.code === OK) {
            this.seller = res.data.data
          }
        })
        .catch(err => {
          console.log(err)
        })
    }
  },
  components: {
    'ele-header': header
  }
}
</script>

<style lang="less" scoped>
@import './common/css/common.less';
#app {
  .tab{
    display: flex;
    height: 40px;
    line-height: 40px;
    .border-1px(rgba(7,17,27,0.1));
    .tabitem{
      text-align: center;
      flex: 1;
      font-size: 14px;
      color: rgb(77,85,93);
      a{
        display: block;
      }
    }
    .router-link-active{
      color: rgb(240,20,20);
    }
  }
}
</style>
