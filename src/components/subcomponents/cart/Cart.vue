<template>
  <div class="cart">
    <div class="content">
      <div class="cartContent" @click="toggleList">
        <div class="cartWrap">
          <div class="cartIcon" :class="{highlight: totalCount > 0}">
            <i ref="cart" class="icon-shopping_cart"></i>
          </div>
          <span v-show="totalCount > 0" class="count">{{totalCount}}</span>
        </div>
        <div class="info">
          <strong class="totalPrice" :class="{highlight: totalPrice > 0}">¥{{totalPrice}}</strong>
          <span class="dlvPrice">另需配送费{{deliveryPrice}}元</span>
        </div>
        <div class="goCheckout" :class="{enough: totalPrice >= minPrice}" @click.stop.prevent="checkout">{{checkoutDesc}}</div>
      </div>
      <div class="ballWrap">
        <transition v-for="(ball, index) in balls" :key="index"
          @before-enter="beforeDrop"
          @enter="dropping"
          @after-enter="afterDrop"
        >
          <div class="ball" v-show="ball.show">
            <div class="inner inner-hook"></div>
          </div>
        </transition>
      </div>
      <transition name="list">
        <div class="cartList" v-show="listShow">
          <div class="listTop">
            <h4 class="title">购物车</h4>
            <span class="clear" @click="clear">清空</span>
          </div>
          <div ref="list" class="foodList">
            <ul>
              <li class="foodItem" v-for="(food, index) in cartList" :key="index">
                <span class="itemName">{{food.name}}</span>
                <p class="price">¥ <strong>{{food.price}}</strong></p>
                <div class="addWrap">
                  <addToCart class="add" :food="food"></addToCart>
                </div>
              </li>
            </ul>
          </div>
        </div>
      </transition>
    </div>
    <transition name="mask">
      <div class="mask" v-show="listShow" @click="toggleList"></div>
    </transition>
  </div>
</template>
<script type="text/javascript">
  import BScroll from 'better-scroll'
  import addToCart from '../../subcomponents/addtocart/AddToCart'
  export default {
    props: {
      cartList: {
        type: Array,
        default: () => {
          return [{price: 10, count: 0}]
        }
      },
      deliveryPrice: Number,
      minPrice: Number
    },
    created () {
      this.createBalls()
    },
    data () {
      return {
        balls: [],
        dropBalls: [],
        fold: true
      }
    },
    methods: {
      toggleList () {
        if (!this.totalCount) {
          this.fold = true
          return
        }
        this.fold = !this.fold
      },
      clear () {
        this.cartList.forEach(item => {
          item.count = 0
        })
      },
      checkout () {
        window.alert(`需支付${this.totalPrice + this.deliveryPrice}元`)
      },
      createBalls () {
        const BALL_LEN = 10
        for (let i = 0; i < BALL_LEN; i++) {
          this.balls.push({show: false})
        }
      },
      drop (target) {
        // 找到第一个隐藏的小球
        const ball = this.balls.find(ball => !ball.show)
        // 显示它，并放入正下落的小球数组
        if (ball) {
          ball.show = true
          ball.startEl = target
          this.dropBalls.push(ball)
        }
      },
      beforeDrop (el) {
        // 拿到第一个小球getBoundingClientRect
        const ball = this.dropBalls.shift()
        // const ball = this.dropBalls[this.dropBalls.length - 1]
        const [left, bottom] = [32, 22]
        const startP = ball.startEl.getBoundingClientRect()
        let dx = startP.left - left
        let dy = -(window.innerHeight - startP.top - bottom)
        // el.style.display = ''
        el.style.transform = `translate3d(0, ${dy}px, 0)`
        el.children[0].style.transform = `translate3d(${dx}px, 0, 0)`
        el.ball = ball
      },
      dropping (el, done) {
        // eslint-disable-next-line
        el.offsetWidth
        // 指定transform样式
        el.style.transform = 'translate3d(0, 0, 0)'
        el.children[0].style.transform = 'translate3d(0, 0, 0)'
        el.addEventListener('transitionend', done)
        // done()
      },
      afterDrop (el) {
        // this.dropBalls.shift().show = false
        el.ball.show = false
        el.style.display = 'none'
      }
    },
    computed: {
      totalPrice () {
        return this.cartList.reduce((preTotal, item) => {
          return preTotal + item.price * item.count
        }, 0)
      },
      totalCount () {
        return this.cartList.reduce((preTotal, item) => {
          return preTotal + item.count
        }, 0)
      },
      checkoutDesc () {
        if (this.totalPrice === 0) {
          return `¥${this.minPrice}起送`
        } else if (this.totalPrice < this.minPrice) {
          return `还差${this.minPrice - this.totalPrice}起送`
        } else {
          return '去结算'
        }
      },
      listShow () {
        if (!this.totalCount) {
          // eslint-disable-next-line
          this.fold = true
          return false
        }
        let show = !this.fold
        if (show) {
          this.$nextTick(() => {
            if (!this.scroll) {
              // eslint-disable-next-line
              this.scroll = new BScroll(this.$refs.list, {
                click: true
              })
            } else {
              this.scroll.refresh()
            }
          })
        }
        return show
        // if (!this.fold) {
        //   return !this.fold
        // }
      }
    },
    components: {
      addToCart
    }
  }
</script>
<style type="text/css" lang="less" scoped>
  @import '../../../common/css/common.less';
  .cart{
    .content{
      position: fixed;
      left: 0;
      bottom: 0;
      // display: flex;
      width: 100%;
      height: 48px;
      z-index: 50;
    }
    .cartContent{
      display: flex;
      line-height: 48px;
      background: #141d27;
      color: rgba(255,255,255,0.4);
    }
    .cartWrap{
      position: relative;
      width: 44px;
      height: 44px;
      flex: 0 0 44px;
      margin: -10px 12px 0;
      border-radius: 50%;
      background: #141d27;
      padding: 6px;
      .cartIcon{
        width: 44px;
        height: 44px;
        border-radius: 50%;
        background: #2b343c;
        text-align: center;
        .icon-shopping_cart{
          content: '\e907';
          font-size: 24px;
          line-height: 44px;
          color: rgba(255,255,255,0.4);
        }
        &.highlight{
          background: rgb(0,160,220);
          .icon-shopping_cart{
            color: #fff;
          }
        }
      }
      .count{
        position: absolute;
        right: 0;
        top: 0;
        padding: 0 6px;
        background: rgb(240,20,20);
        border-radius: 8px;
        box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4);
        font-size: 9px;
        color: #fff;
        line-height: 16px;
      }
    }
    .info{
      flex: 1;
      .totalPrice{
        height: 24px;
        padding-right: 12px;
        border-right: 1px solid rgba(255,255,255,0.1);
        margin: 12px 10px 0 0;
        font-size: 16px;
        line-height: 24px;
        font-weight: 700;
        &.highlight{
          color: #fff;
        }
      }
      .dlvPrice{
        font-size: 14px;
      }
    }
    .goCheckout{
      width: 105px;
      height: 48px;
      flex: 0 0 105px;
      background: #2b343c;
      text-align: center;
      font-size: 12px;
      font-weight: 700;
      &.enough{
        background: #00b43c;
        color: #fff;
      }
    }
    .ball{
      position: fixed;
      left: 32px;
      bottom: 22px;
      transition: all 0.4s cubic-bezier(0.44,-0.25,0.8,0.55);
      z-index: 200;
      .inner{
        width: 16px;
        height: 16px;
        border-radius: 50%;
        background: rgb(0,160,220);
        transition: all 0.4s linear;
      }
    }
    .cartList{
      position: absolute;
      left: 0;
      top: 0;
      width: 100%;
      z-index: -1;
      transform: translate3d(0, -100%, 0);
      &.list-enter, &.list-leave-to{
        transform: translate3d(0, 0, 0);
      }
      &.list-enter-active, &.list-leave-active{
        transition: all 0.3s linear;
      }
      .listTop{
        height: 40px;
        background: #f3f5f7;
        padding: 0 18px;
        line-height: 40px;
        font-size: 14px;
        font-weight: 200;
        color: rgb(7,17,27);
        .border-1px(rgba(7,17,27,0.1));
        box-sizing: border-box;
        .title{
          float: left;
        }
        .clear{
          float: right;
          font-size: 12px;
          color: rgb(0,160,220);
        }
      }
      .foodList{
        max-height: 222px;
        background: #fff;
        overflow: hidden;
        // overflow-y: scroll;
        // &::-webkit-scrollbar {
        //   display:none
        // }
        .foodItem{
          height: 48px;
          padding: 0 14px 0 18px;
          line-height: 48px;
          font-size: 14px;
          color: rgb(7,17,27);
          overflow: hidden;
          display: flex;
          .border-1px(rgba(7,17,27,0.1));
          box-sizing: border-box;
          .itemName{
            flex: 1;
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
          }
          .price{
            font-size: 10px;
            color: rgb(240,20,20);
            vertical-align: bottom;
            strong{
              font-size: 14px;
              font-weight: 700;
            }
          }
          .addWrap{
            min-width: 79px;
            margin: 8px 0 0 12px;
            .add{
              float: right;
            }
          }
        }
      }
    }
    .mask{
      position: fixed;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      z-index: 40;
      background: rgba(7,17,27,0.6);
      backdrop-filter: blur(10px);
      &.mask-enter, &.mask-leave-to{
        opacity: 0;
      }
      &.mask-enter-active, &.mask-leave-active{
        transition: all 0.3s linear;
      }
    }
  }
</style>
