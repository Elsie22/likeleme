<template>
<div class="add">
  <transition name="re">
    <i ref="reBtn" class="icon icon-remove_circle_outline" v-show="food.count>0" @click.stop.prevent="reduce"></i>
  </transition>
  <span class="count" v-show="food.count>0">{{food.count}}</span>
  <i class="icon icon-add_circle" @click.stop.prevent="add($event)" ref="addBtn"></i>
</div>
</template>
<script>
  import Vue from 'vue'
  const ADD_EVT = 'addItem'
  export default {
    props: ['food'],
    methods: {
      add (event) {
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
        this.$emit(ADD_EVT, event.target)
      },
      reduce () {
        this.food.count--
      }
    },
    computed: {
    }
    /* mounted () {
      console.log(this.$refs.reBtn.getBoundingClientRect())
      console.log(this.$refs)
    } */
  }
</script>
<style type="text/javascript" lang="less" scoped>
.add{
  height: 32px;
  line-height: 32px;
  .icon{
    float: left;
    padding: 4px;
    font-size: 24px;
    color: rgb(0,160,220);
  }
  .icon-add_circle{
    content: '\e900';
  }
  .icon-remove_circle_outline{
    content: '\e906';
  }
  .count{
    font-size: 10px;
    text-align: center;
    float: left;
    display: inline-block;
  }
   .re-enter, .re-leave-to{
    opacity: 0;
    transform: translateX(22px) rotate(180deg);
  }
  .re-enter-active, .re-leave-active{
    transition: all 0.3s ease;
  }
  .ball{
    position: absolute;
    right: 8px;
    bottom: 8px;
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: rgb(0,160,220);
  }
}
</style>
