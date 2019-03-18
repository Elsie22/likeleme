<template>
<div class="goods">
  <!-- <div class="listWrap"> -->
    <div class="goodsWrap" ref="mnlist">
      <ul class="menuList">
        <li class="menuItem" v-for="(item, index) in goods" :key="index" :class="{active: (index === currentIndex)}" @click="clickMenu(index)">
          <div class="item">
            <supporticon class="spticon" v-if="(item.type > 0)" :size="24" :iconNo="item.type" :iconType="3"></supporticon>
            <span class="text">{{item.name}}</span>
          </div>
        </li>
      </ul>
    </div>
    <div class="foodsWrap" ref="fdlist">
      <ul class="goodsList">
        <li class="goodsItem goods-item-hook" v-for="(item, index) in goods" :key="index">
          <h4 class="colName">{{item.name}}</h4>
          <ul class="foods">
            <li class="foodItem" v-for="(food, index) in item.foods" :key="index" @click="showDetail(food)">
              <img class="pic" :src="food.icon">
              <div class="foodInfo">
                <h6 class="name">{{food.name}}</h6>
                <p class="descp" v-if="(food.description.length > 0)">{{food.description}}</p>
                <p class="amount">
                  <span class="monthCount">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </p>
                <p class="price">
                  <span class="nowPrice">¥<strong>{{food.price}}</strong></span>
                  <span class="oldPrice" v-if="(food.oldPrice)">¥<strong>{{food.oldPrice}}</strong></span>
                </p>
              </div>
              <div class="addWrap">
                <addtocart :food="food" @addItem="onadd"></addtocart>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  <!-- </div> -->
	<cart ref="cart" :deliveryPrice="seller.deliveryPrice" :minPrice="seller.minPrice" :cartList="cartList"></cart>
  <foodetail ref="foodDetail" :food="food" @addItem="onadd"></foodetail>
</div>
</template>

<script type="text/javascript">
  import axios from 'axios'
  import BScroll from 'better-scroll'
  import supporticon from '../subcomponents/supporticon/Supporticon'
  import cart from '../subcomponents/cart/Cart'
  import addtocart from '../subcomponents/addtocart/AddToCart'
  import foodetail from './FoodDetail'
  const OK = 0
  export default{
    props: ['seller'],
    data () {
      return {
        goods: [],
        // currentIndex: 0,
        foodScrollY: 0,
        fdHList: [],
        detailShow: false,
        food: {}
      }
    },
    created () {
      axios.get('/api/goods')
        .then(res => {
          if (res.data.code === OK) {
            this.goods = res.data.data
            this.$nextTick(() => {
              this.initScroll()
              this.calfdHeight()
            })
          }
        })
        .catch(err => {
          console.log(err)
        })
    },
    methods: {
      initScroll () {
        // eslint-disable-next-line
        this.mnScroll = new BScroll(this.$refs.mnlist,{
          click: true
        })
        // eslint-disable-next-line
        this.fdScroll = new BScroll(this.$refs.fdlist,{
          probeType: 3,
          click: true
        })
        this.fdScroll.on('scroll', (pos) => {
          this.foodScrollY = Math.abs(Math.round(pos.y))
        })
      },
      clickMenu (index) {
        let fdLi = this.$refs.fdlist.getElementsByClassName('goods-item-hook')[index]
        this.fdScroll.scrollToElement(fdLi, 300)
      },
      calfdHeight () {
        let height = 0
        this.fdHList.push(height)
        let goodsItems = this.$refs.fdlist.getElementsByClassName('goods-item-hook')
        Array.from(goodsItems).forEach(item => {
          height += item.clientHeight
          this.fdHList.push(height)
        })
      },
      onadd (target) {
        this.$refs.cart.drop(target)
      },
      showDetail (food) {
        this.food = food
        this.$refs.foodDetail.show()
      }
    },
    computed: {
      currentIndex () {
        for (let i = 0; i < this.fdHList.length; i++) {
          if (this.fdHList[i + 1] && this.foodScrollY >= this.fdHList[i] && this.foodScrollY < this.fdHList[i + 1]) {
            return i
          }
        }
        return 0
      },
      cartList () {
        let selFoods = []
        this.goods.forEach(good => {
          good.foods.forEach(food => {
            if (food.count > 0) {
              selFoods.push(food)
            }
          })
        })
        return selFoods
      }
    },
    components: {
      supporticon,
      cart,
      addtocart,
      foodetail
    }
  }
</script>

<style type="text/css" lang="less" scoped>
@import '../../common/css/common.less';
.goods{
  display: flex;
  position: absolute;
  top: 174px;
  bottom: 46px;
  width: 100%;
  overflow: hidden;
  //.listWrap{
    .goodsWrap{
      width: 80px;
      flex: 0 0 80px;
      background: #f3f5f7;
      .menuItem{
        display: table;
        width: 100%;
        height: 54px;
        padding: 0 12px;
        box-sizing: border-box;
        font-size: 12px;
        .item{
          display: table-cell;
          vertical-align: middle;
          line-height: 14px;
          .border-1px(rgba(7,17,27,0.1));
          .spticon{
            display: inline-block;
          }
        }
        &.active{
          position: relative;
          background: #fff;
          margin-top: -1px;
          z-index: 1;
          .item{
            .text{
              font-weight: 700;
            }
            &:after{
              border: 0;
            }
          }
        }
        &:last-child{
          .item:after{
            border: 0;
          }
        }
      }
    }
    .foodsWrap{
      flex: 1;
      width: 100%;
      overflow: hidden;
      .goodsItem{
        .colName{
          height: 36px;
          padding-left: 12px;
          border-left: 2px solid #d9dde1;
          font-size: 12px;
          color: rgb(147,153,159);
          font-weight: normal;
          line-height: 36px;
          background: #f3f5f7
        }
        .foods{
          background: #fff;
          padding: 0 18px;
          .foodItem{
            display: flex;
            padding: 18px 0;
            overflow: hidden;
            .border-1px(rgba(7,17,27,0.1));
            .pic{
              width: 57px;
              height: 57px;
              flex: 0 0 57px;
              border-radius: 2px;
              margin-right: 10px;
            }
            .foodInfo{
              flex: 1;
              overflow: hidden;
              color: rgb(147,153,159);
              font-size: 10px;
              .name{
                overflow: hidden;
                white-space: nowrap;
                text-overflow: ellipsis;
                margin-top: 2px;
                font-size: 14px;
                color: rgb(7,17,27);
                line-height: 14px;
              }
              .descp{
                overflow: hidden;
                white-space: nowrap;
                text-overflow: ellipsis;
                line-height: 12px;
                font-size: 10px;
                margin-top: 7px;
              }
              .amount{
                line-height: 10px;
                margin-top: 7px;
                .monthCount{
                  margin-right: 12px;
                }
              }
              .price{
                line-height: 24px;
                .nowPrice{
                  margin-right: 8px;
                  color: rgb(240,20,20);
                  strong{
                    font-weight: bold;
                    font-size: 14px;
                  }
                }
                .oldPrice{
                  text-decoration: line-through;
                  strong{
                    font-weight: bold;
                  }
                }
              }
            }
            .addWrap{
              position: absolute;
              right: 0;
              bottom: 14px;
            }
            &:last-child{
              &:after{
                border: 0 !important;
              }
            }
          }
        }
      }
    }
  // }
  // .cart{
    // position: fixed;
    // bottom: 0;
    // width: 100%;
    // height: 48px;
    // z-index: 99;
  // }
}
</style>
