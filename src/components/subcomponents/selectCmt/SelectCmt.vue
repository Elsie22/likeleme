<template>
  <div class="selectCmt">
    <div class="choices">
      <span @click="select(2)" class="choice" :class="{'highlight':selectType===2}">{{choices[0]}}
        <span class="count">{{ratings.length}}</span>
      </span>
      <span @click="select(0)" class="choice" :class="{'highlight':selectType===0}">{{choices[1]}}
        <span class="count">{{positives.length}}</span>
      </span>
      <span @click="select(1)" class="choice negative" :class="{'highlight':selectType===1}">{{choices[2]}}
        <span class="count">{{negatives.length}}</span>
      </span>
    </div>
    <div class="only">
      <i @click="toggle" class="icon icon-check_circle" :class="{'highlight': onlyCont}"></i>
      <span>只看有内容的评价</span>
    </div>
  </div>
</template>
<script type="text/javascript">
  const POSITIVE = 0
  const NEGATIVE = 1
  const ALL = 2
  export default {
    props: {
      choices: {
        type: Array,
        default: () => {
          return ['全部', '推荐', '吐槽']
        }
      },
      ratings: {
        type: Array,
        default: () => {
          return []
        }
      },
      onlyCont: {
        type: Boolean,
        default: false
      },
      selectType: {
        type: Number,
        default: ALL
      }
    },
    methods: {
      select (type) {
        this.$emit('select', type)
      },
      toggle () {
        this.$emit('toggle')
      }
    },
    computed: {
      positives () {
        return this.ratings.filter(item => {
          return item.rateType === POSITIVE
        })
      },
      negatives () {
        return this.ratings.filter(item => {
          return item.rateType === NEGATIVE
        })
      }
    }
  }
</script>

<style type="text/css" lang="less" scoped>
  @import '../../../common/css/common.less';
  .selectCmt{
    .choices{
      padding: 18px 0;
      .border-1px(rgba(7,17,27,0.1));
      .choice{
        display: inline-block;
        margin-right: 8px;
        padding: 8px 12px;
        border-radius: 1px;
        background: rgba(0,160,220,0.2);
        font-size: 12px;
        line-height: 16px;
        color: rgb(77,85,93);
        .count{
          font-size: 8px;
        }
        &.highlight{
          background: rgb(0,160,220);
          color: #fff;
        }
        &.negative{
          background: rgba(77,85,93,0.2);
          &.highlight{
            background: rgba(77,85,93,1);
            color: #fff;
          }
        }
      }
    }
    .only{
      overflow: hidden;
      line-height: 24px;
      color: rgb(147,153,159);
      font-size: 12px;
      .icon{
        float: left;
        padding: 12px 4px 12px 6px;
        font-size: 24px;
        color: rgb(147,153,159);
        &.highlight{
          color: #00c850;
        }
      }
      span{
        float: left;
        padding-top: 12px;
      }
    }
  }
</style>
