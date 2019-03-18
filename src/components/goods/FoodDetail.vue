<template>
  <transition name="detail">
    <div class="detail" v-show="myShow">
      <div class="imgWrap">
        <img class="image" :src="food.image">
        <i class="back icon-arrow_lift" @click="back"></i>
      </div>
      <div class="foodInfo">
        <h4 class="name">{{food.name}}</h4>
        <p class="count"><span class="monthCount">月售{{food.sellCount}}份</span>好评率{{food.rating}}%</p>
        <p class="price">
          <span class="nowPrice">¥<strong>{{food.price}}</strong></span>
          <span class="oldPrice" v-if="food.oldPrice">¥<strong>{{food.oldPrice}}</strong></span>
        </p>
        <transition name="fade">
          <div class="addBtn" v-show="!food.count || food.count === 0" @click.stop.prevent="newAdd($event)">加入购物车</div>
        </transition>
        <div class="addWrap" v-show="food.count">
          <addtocart :food="food" @addItem="add"></addtocart>
        </div>
      </div>
      <split></split>
      <div class="intro">
        <h4 class="title">商品介绍</h4>
        <p class="info">{{food.info}}</p>
      </div>
      <split></split>
      <div class="comments">
        <h4 class="title">商品评价</h4>
        <selectcmt v-show="food.ratings && food.ratings.length > 0" class="selectCmt" :onlyCont="onlyCont" :ratings="food.ratings" :selectType="selectType" :choices="choices" @toggle="toggle" @select="select">
          </selectcmt>
        <div class="cmmtList" v-show="food.ratings && food.ratings.length > 0">
          <ul>
            <li class="cmmtItem" v-for="(cmmt, index) in slctRatings" :key="index">
              <div class="timeWrap">
                <span class="time">{{cmmt.rateTime | dateFormat('yy-MM-dd hh:mm')}}</span>
                <div class="user">
                  <span>{{cmmt.username}}</span>
                  <img class="avatar" :src="cmmt.avatar">
                </div>
              </div>
              <p class="cmmtCont">
                <i class="icon" :class="cmmt.rateType?'icon-thumb_down':'icon-thumb_up'"></i>
                <span class="text" v-show="cmmt.text">{{cmmt.text}}</span>
              </p>
            </li>
          </ul>
        </div>
        <div class="noCmmt" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
      </div>
    </div>
  </transition>
</template>
<script type="text/javascript">
  import Vue from 'vue'
  import split from '../subcomponents/split/Split'
  import selectcmt from '../subcomponents/selectCmt/SelectCmt'
  import addtocart from '../subcomponents/addtocart/AddToCart'
  // eslint-disable-next-line
  const POSITIVE = 0
  // eslint-disable-next-line
  const NEGATIVE = 1
  const ALL = 2
  const ADD_EVT = 'addItem'
  export default {
    props: ['food'],
    data () {
      return {
        myShow: false,
        onlyCont: false,
        selectType: ALL,
        choices: ['全部', '推荐', '吐槽']
      }
    },
    watch: {
      selectType: function () {
        // console.log(this.selectType)
        // console.log(this.slctRatings)
      }
    },
    methods: {
      show () {
        this.myShow = true
      },
      back () {
        this.myShow = false
      },
      select (type) {
        this.selectType = type
      },
      toggle () {
        this.onlyCont = !this.onlyCont
      },
      newAdd (event) {
        if (!this.food.count) {
          Vue.set(this.food, 'count', 1)
        } else {
          this.food.count++
        }
        this.$emit(ADD_EVT, event.target)
      },
      add (target) {
        this.$emit(ADD_EVT, target)
      }
    },
    computed: {
      slctRatings () {
        if (!this.food.ratings) {
          return
        }
        return this.food.ratings.filter(item => {
          if (this.selectType === ALL) {
            return this.onlyCont ? !!item.text : true
          }
          return item.rateType === this.selectType && (this.onlyCont ? !!item.text : true)
        })
      }
    },
    components: {
      split,
      selectcmt,
      addtocart
    }
  }
</script>
<style type="text/css" lang="less" scoped>
  @import '../../common/css/common.less';
  .detail-enter, .detail-leave-to{
    transform: translateX(100%);
  }
  .detail-enter-active, .detail-leave-active{
    transition: all 0.3s ease;
  }
  .detail{
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: #fff;
    z-index: 30;
    overflow-y: scroll;
    .imgWrap{
      position: relative;
      width: 100%;
      padding-top: 100%;
      .image{
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
      }
      .back{
        position: absolute;
        left: 10px;
        top: 10px;
        padding: 8px;
        color: #fff;
        font-size: 18px;
      }
    }
    .foodInfo{
      position: relative;
      padding: 18px 18px;
      .fade-enter, .fade-leave-to{
        opacity: 0;
      }
      .fade-enter-active, .fade-leave-active{
        transition: all 0.3s linear;
      }
      .name{
        font-size: 14px;
        font-weight: 700;
        color: rgb(7,17,27);
        line-height: 14px;
      }
      .count{
        color: rgb(147,153,159);
        font-size: 12px;
        line-height: 12px;
        margin-top: 7px;
        .monthCount{
          margin-right: 12px;
        }
      }
      .price{
        font-size: 10px;
        line-height: 24px;
        margin-top: 18px;
        .nowPrice{
          color: rgb(240,20,20);
          strong{
            font-size: 14px;
            font-weight: 700;
          }
        }
        .oldPrice{
          color: rgb(147,153,159);
          text-decoration: line-through;
          strong{
            font-weight: 700;
          }
        }
      }
      .addBtn{
        position: absolute;
        right: 18px;
        bottom: 18px;
        height: 24px;
        border-radius: 12px;
        background: rgb(0,160,220);
        font-size: 10px;
        color: #fff;
        line-height: 12px;
        text-align: center;
        padding: 6px 12px;
        box-sizing: border-box;
      }
      .addWrap{
        position: absolute;
        right: 18px;
        bottom: 14px;
      }
    }
    .intro{
      padding: 18px;
      .title{
        font-size: 14px;
        color: rgb(7,17,27);
        line-height: 14px;
      }
      .info{
        font-size: 12px;
        color: rgb(77,85,93);
        line-height: 24px;
        padding: 6px 8px 0;
      }
    }
    .comments{
      padding-top: 18px;
      padding-bottom: 48px;
      .title{
        padding: 0 18px;
        font-size: 14px;
        color: rgb(7,17,27);
        line-height: 14px;
      }
      .selectCmt{
        padding: 0 18px;
        .border-1px(rgba(7,17,27,0.1));
      }
      .cmmtList{
        padding: 0 18px;
        .cmmtItem{
          padding: 16px 0;
          .border-1px(rgba(7,17,27,0.1));
          .timeWrap{
            position: relative;
            font-size: 10px;
            line-height: 12px;
            overflow: hidden;
            color: rgb(147,153,159);
            .time{
              font-size: 10px;
              line-height: 12px;
            }
            .user{
              position: absolute;
              right: 0;
              top: 0;
              .avatar{
                width: 12px;
                height: 12px;
                border-radius: 50%;
                margin-left: 6px;
              }
            }
          }
          .cmmtCont{
            margin-top: 6px;
            font-size: 12px;
            line-height: 16px;
            color: rgb(7,17,27);
            .icon{
              margin-right: 4px;
            }
            .icon-thumb_down{
              color: rgb(147,153,159);
            }
            .icon-thumb_up{
              color: rgb(0,160,220);
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
