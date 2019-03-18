<template>
<div class="seller" ref="seller">
  <div class="sellerInner">
    <div class="wrap intro">
      <h2 class="title">{{seller.name}}</h2>
      <div class="stars">
        <star :size="36" :score="seller.score"></star>
        <span class="ratingCount">({{seller.ratingCount}})</span>
        <span class="sellCount">月售{{seller.sellCount}}单</span>
      </div>
      <ul class="nums">
        <li class="numItem">
          <span class="itemTitle">起送价</span>
          <p>
            <strong class="itemBig">{{seller.minPrice}}</strong>
            <span class="itemSmall">元</span>
          </p>
        </li>
        <li class="numItem">
          <span class="itemTitle">配送费</span>
          <p>
            <strong class="itemBig">{{seller.deliveryPrice}}</strong>
            <span class="itemSmall">元</span>
          </p>
        </li>
        <li class="numItem">
          <span class="itemTitle">平均配送时间</span>
          <p class="itemNum">
            <strong class="itemBig">{{seller.deliveryTime}}</strong>
            <span class="itemSmall">分钟</span>
          </p>
        </li>
      </ul>
      <div class="like" @click="toggleLike">
        <i class="icon icon-favorite" :class="{'liked': liked}"></i>
        <span class="likeText">{{likeText}}</span>
      </div>
    </div>
    <split></split>
    <div class="wrap activities">
      <h4 class="title">公告与活动</h4>
      <p class="notice">{{seller.bulletin}}</p>
      <ul class="actvList">
        <li class="actvItem" v-for="(active, index) in seller.supports" :key="index">
          <spticon class="icon" :iconType="4" :size="32" :iconNo="active.type"></spticon>
          <span class="desc">{{active.description}}</span>
        </li>
      </ul>
    </div>
    <split></split>
    <div class="wrap pics">
      <h4 class="title">商家实景</h4>
      <div class="picListWrap" ref="picWrap">
        <ul class="picList" ref="picUl">
          <li class="pic" v-for="(img, index) in seller.pics" :key="index">
            <img class="picImg" :src="img">
          </li>
        </ul>
      </div>
    </div>
    <split></split>
    <div class="wrap sellerInfo">
      <h4 class="title">商家信息</h4>
      <ul class="infoList">
        <li class="infoItem" v-for="(info, index) in seller.infos" :key="index">{{info}}</li>
      </ul>
    </div>
  </div>
</div>
</template>

<script type="text/javascript">
  import BScroll from 'better-scroll'
  import star from '../subcomponents/star/Star'
  import split from '../subcomponents/split/Split'
  import spticon from '../subcomponents/supporticon/Supporticon'
  export default{
    props: ['seller'],
    data () {
      return {
        liked: false
      }
    },
    computed: {
      likeText () {
        return this.liked ? '已收藏' : '收藏'
      }
    },
    created () {
      this.liked = localStorage.getItem('liked')
    },
    methods: {
      toggleLike () {
        this.liked = !this.liked
        localStorage.setItem('liked', this.liked)
      },
      initScroll () {
        if (!this.scroll) {
          this.scroll = new BScroll(this.$refs.seller, {
            click: true
          })
        } else {
          this.scroll.refresh()
        }
      },
      initPicScroll () {
        if (this.seller.pics) {
          let picWidth = 120
          let margin = 6
          let ulWidth = (picWidth + margin) * this.seller.pics.length - margin
          this.$refs.picUl.style.width = `${ulWidth}px`
          if (!this.picScroll) {
            this.picScroll = new BScroll(this.$refs.picWrap, {
              scrollX: true,
              eventPassthrough: 'vertical'
            })
          } else {
            this.picScroll.refresh()
          }
        }
      }
    },
    mounted () {
      this.$nextTick(() => {
        this.initScroll()
        this.initPicScroll()
      })
    },
    watch: {
      'seller' () {
        this.$nextTick(() => {
          this.initScroll()
          this.initPicScroll()
        })
      }
    },
    components: {
      star,
      split,
      spticon
    }
  }
</script>

<style type="text/css" lang="less" scoped>
@import '../../common/css/common.less';
.seller{
  background: #fff;
  position: absolute;
  top: 174px;
  bottom: 0;
  width: 100%;
  overflow: hidden;
  // overflow-y: scroll;
  .wrap{
    padding: 18px;
    .title{
      font-size: 14px;
      line-height: 14px;
      color: rgb(7,17,27);
      font-weight: 540;
    }
  }
  .intro{
    position: relative;
    .stars{
      padding: 8px 0 18px;
      font-size: 10px;
      color: rgb(77,85,93);
      line-height: 18px;
      .border-1px(rgba(7,17,27,0.1));
      .star{
        display: inline-block;
        vertical-align: top;
      }
      .ratingCount{
        display: inline-block;
        vertical-align: top;
        margin-left: 8px;
      }
      .sellCount{
        display: inline-block;
        vertical-align: top;
        margin-left: 12px;
      }
    }
    .nums{
      display: flex;
      padding-top: 18px;
      .numItem{
        flex: 1;
        text-align: center;
        border-right: 1px solid rgba(7,17,27,0.1);
        &:last-child{
          border: 0;
        }
        .itemTitle{
          font-size: 10px;
          color: rgb(147,153,159);
          line-height: 10px;
        }
        .itemNum{
          margin-top: 4px;
        }
        .itemBig{
          font-size: 24px;
          color: rgb(7,17,27);
          line-height: 24px;
        }
        .itemSmall{
          font-size: 10px;
        }
      }
    }
    .like{
      position: absolute;
      right: 18px;
      top: 20px;
      width: 36px;
      text-align: center;
      .icon{
        display: block;
        font-size: 24px;
        color: rgba(147,153,159,0.5);
        line-height: 24px;
        &.liked{
          color: rgb(240,20,20);
        }
      }
      .likeText{
        display: block;
        margin-top: 4px;
        font-size: 10px;
        color: rgb(77,85,93);
        line-height: 10px;
      }
    }
  }
  .activities{
    padding-bottom: 0;
    .notice{
      margin-top: 8px;
      font-size: 12px;
      font-weight: 200;
      color: rgb(240,20,20);
      line-height: 24px;
    }
    .actvList{
      margin-top: 16px;
      .actvItem{
        padding: 16px 12px;
        border-top: 1px solid rgba(7,17,27,0.1);
        .icon{
          display: inline-block;
          vertical-align: top;
        }
        .desc{
          display: inline-block;
          vertical-align: top;
          margin-left: 6px;
          font-size: 12px;
          line-height: 16px;
          color: rgb(7,17,27);
        }
      }
    }
  }
  .pics{
    .picListWrap{
      margin-top: 12px;
      width: 100%;
      overflow: hidden;
    }
    .picList{
      display: flex;
      .pic{
        width: 120px;
        height: 90px;
        margin-right: 6px;
        &:last-child{
          margin-right: 0;
        }
        .picImg{
          width: 120px;
          height: 90px;
        }
      }
    }
  }
  .sellerInfo{
    padding-bottom: 0;
    .infoList{
      margin-top: 12px;
      .infoItem{
        padding: 16px 12px;
        font-size: 12px;
        color: rgb(7,17,27);
        line-height: 16px;
        border-top: 1px solid rgba(7,17,27,0.1);
      }
    }
  }
}
</style>
