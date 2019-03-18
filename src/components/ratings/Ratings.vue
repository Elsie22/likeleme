<template>
<div class="ratings">
	<div class="score">
   <div class="scoreAll">
     <p class="scoreSl">{{seller.score}}</p>
     <p class="zhRate">综合评分</p>
     <p class="higher">高于周边商家{{seller.rankRate}}%</p>
   </div>
   <div class="scoreDetail">
      <p class="scoreWrap">
        <span class="title">商品评分</span>
        <star :score="seller.foodScore" :size="36"></star>
        <span class="detailScore">{{seller.foodScore}}</span>
      </p>
      <p class="scoreWrap">
        <span class="title">服务态度</span>
        <star :score="seller.serviceScore" :size="36"></star>
        <span class="detailScore">{{seller.serviceScore}}</span>
      </p>
      <p class="scoreWrap">
        <span class="title">送达时间</span>
        <span class="ariveTime">{{seller.deliveryTime}}分钟</span>
      </p>
   </div>
  </div>
  <split></split>
  <div class="cmmtWrap">
    <selectcmt v-show="ratings && ratings.length > 0" class="selectCmt" :onlyCont="onlyCont" :ratings="ratings" :selectType="selectType" :choices="choices" @toggle="toggle" @select="select"></selectcmt>
    <div class="cmmtList" v-show="ratings && ratings.length > 0">
      <ul>
        <li class="cmmtItem" v-for="(item, index) in slctRatings" :key="index">
          <img class="avatar" :src="item.avatar">
          <div class="cmmtDetail">
            <p class="user">
              <span class="name">{{item.username}}</span>
              <span class="rateTime">{{item.rateTime | dateFormat('yyyy-MM-dd hh:mm')}}</span>
            </p>
            <div class="starInfo">
              <star :score="item.score" :size="24"></star>
              <span class="ariveTime">{{item.deliveryTime}}分钟送达</span>
            </div>
            <p class="content">{{item.text}}</p>
            <div class="recommend" v-if="item.recommend && item.recommend.length > 0">
              <i class="icon icon-thumb_up"></i>
              <span class="rcmdItem" v-for="(rcmd, index) in item.recommend" :key="index">{{rcmd}}</span>
            </div>
            <div class="oppose recommend" v-if="item.oppose && item.oppose.length > 0">
              <i class="icon icon-thumb_down"></i>
              <span class="rcmdItem" v-for="(opsItem, index) in item.oppose" :key="index">{{opsItem}}</span>
            </div>
          </div>
        </li>
      </ul>
    </div>
    <div class="noCmmt" v-show="!ratings || !ratings.length">暂无评价</div>
  </div>
</div>
</template>

<script type="text/javascript">
  import axios from 'axios'
  import star from '../subcomponents/star/Star'
  import split from '../subcomponents/split/Split'
  import selectcmt from '../subcomponents/selectCmt/SelectCmt'
  // eslint-disable-next-line
  const POSITIVE = 0
  // eslint-disable-next-line
  const NEGATIVE = 1
  const ALL = 2
  const OK = 0
  export default{
    props: ['seller'],
    data () {
      return {
        ratings: [],
        choices: ['全部', '满意', '不满意'],
        onlyCont: true,
        selectType: ALL
      }
    },
    methods: {
      select (type) {
        this.selectType = type
      },
      toggle () {
        this.onlyCont = !this.onlyCont
      }
    },
    created () {
      axios.get('/api/ratings')
        .then(res => {
          if (res.data.code === OK) {
            this.ratings = res.data.data
            // this.$nextTick(() => {
            //   this.initScroll()
            //   this.calfdHeight()
            // })
          }
        })
        .catch(err => {
          console.log(err)
        })
    },
    computed: {
      slctRatings () {
        if (!this.ratings) {
          return
        }
        return this.ratings.filter(item => {
          if (this.selectType === ALL) {
            return this.onlyCont ? !!item.text : true
          }
          return item.rateType === this.selectType && (this.onlyCont ? !!item.text : true)
        })
      }
    },
    components: {
      star,
      split,
      selectcmt
    }
  }
</script>

<style type="text/css" lang="less" scoped>
@import '../../common/css/common.less';
.ratings{
  position: absolute;
  top: 174px;
  bottom: 0;
  width: 100%;
  overflow-y: scroll;
  .score{
    display: flex;
    padding: 18px 0;
    .scoreAll{
      width: 137px;
      padding: 6px 0;
      flex: 0 0 137px;
      text-align: center;
      border-right: 1px solid rgba(7,17,27,0.1);
      @media only screen and (max-width: 320px){
        flex: 0 0 120px;
        width: 120px;
      }
      .scoreSl{
        font-size: 24px;
        color: rgb(255,153,0);
        line-height: 28px;
      }
      .zhRate{
        margin-top: 6px;
        font-size: 12px;
        color: rgb(7,17,27);
        line-height: 12px;
      }
      .higher{
        margin-top: 8px;
        font-size: 10px;
        color: rgb(147,153,159);
        line-height: 10px;
      }
    }
    .scoreDetail{
      padding: 6px 0;
      flex: 1;
      padding-left: 20px;
      @media only screen and (max-width: 320px){
        padding-left: 6px;
      }
      .scoreWrap{
        // line-height: 18px;
        margin-top: 8px;
        &:first-child{
          margin-top: 0;
        }
        .title{
          display: inline-block;
          vertical-align: top;
          margin-right: 4px;
          font-size: 12px;
          line-height: 18px;
          color: rgb(7,17,27);
          @media only screen and (max-width: 320px){
            margin-right: 0;
          }
        }
        .star{
          display: inline-block;
          vertical-align: top;
          margin-right: 6px;
          @media only screen and (max-width: 320px){
            margin-right: 0;
          }
        }
        .detailScore{
          display: inline-block;
          vertical-align: top;
          font-size: 12px;
          line-height: 18px;
          color: rgb(255,153,0);
        }
        .ariveTime{
          font-size: 12px;
          color: rgb(147,153,159);
        }
      }
    }
  }
  .cmmtWrap{
    .selectCmt{
      padding: 0 18px;
      .border-1px(rgba(7,17,27,0.1));
    }
    .cmmtList{
      padding: 0 18px;
      .cmmtItem{
        padding: 18px;
        display: flex;
        .border-1px(rgba(7,17,27,0.1));
        &:last-child{
          &:after{
            border: 0;
          }
        }
        .avatar{
          width: 28px;
          flex: 0 0 28px;
          height: 28px;
          border-radius: 50%;
        }
        .cmmtDetail{
          flex: 1;
          margin-left: 12px;
          .user{
            font-size: 10px;
            line-height: 12px;
            overflow: hidden;
            .name{
              float: left;
              color: rgb(7,17,27);
            }
            .rateTime{
              float: right;
              color: rgb(147,153,159);
            }
          }
          .starInfo{
            height: 12px;
            margin-top: 4px;
            line-height: 12px;
            overflow: hidden;
            .star{
              float: left;
              margin-right: 6px;
            }
            .ariveTime{
              float: left;
              font-size: 10px;
              color: rgb(147,153,159);
            }
          }
          .content{
            margin-top: 6px;
            font-size: 12px;
            line-height: 18px;
            color: rgb(7,17,27);
          }
          .recommend{
            margin-top: 8px;
            .icon{
              font-size: 12px;
              line-height: 16px;
              color: rgb(0,160,220);
            }
            .rcmdItem{
              display: inline-block;
              padding: 0 6px;
              margin: 0 8px 4px 0;
              border-radius: 1px;
              border: 1px solid rgba(7,17,27,0.1);
              font-size: 9px;
              color: rgb(147,153,159);
              line-height: 16px;
            }
          }
          .oppose{
            .icon{
              color: rgb(183,187,191);
            }
          }
        }
      }
    }
    .noCmmt{
      font-size: 12px;
      color: rgb(147,153,159);
      line-height: 48px;
      padding-left: 18px;
    }
  }
}
</style>
