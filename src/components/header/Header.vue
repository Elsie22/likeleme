<template>
<div class="header">
  <div class="content-wrap">
    <div class="sign">
      <img :src="seller.avatar">
    </div>
    <div class="intro">
      <div class="title">
        <span class="brand"></span>
        <span class="name"> {{seller.name}} </span>
      </div>
      <div class="description">{{seller.description}} / {{seller.deliveryTime}}分钟送达</div>
      <div class="supports" v-if="seller.supports">
        <supporticon class="spticon" :size="24" :iconNo="seller.supports[0].type" :iconType="2"></supporticon>
        <span class="desp">{{seller.supports[0].description}}</span>
      </div>
    </div>
  </div>
  <div class="sptCount" v-if="seller.supports" @click="showDetail">
    <span class="count">{{seller.supports.length}}个</span>
    <i class="icon-keyboard_arrow_right"></i>
  </div>
  <div class="bulletin" @click="showDetail">
    <span class="bulletbg"></span>
    <span class="bullettext">{{seller.bulletin}}</span>
    <i class="icon-keyboard_arrow_right"></i>
  </div>
  <div class="bg">
    <img :src="seller.avatar">
  </div>
  <transition name="detail">
    <div class="detail" v-show="detailshow">
      <div class="wrap">
        <h4>{{seller.name}}</h4>
        <div class="starWrap">
          <star :score="seller.score" :size="48"></star>
        </div>
        <div class="supt" v-if="seller.supports">
          <div class="title">
            <span class="line"></span>
            <h6>优惠信息</h6>
            <span class="line"></span>
          </div>
          <ul class="suptlist">
            <li v-for="(item, index) in seller.supports" :key="index">
              <supporticon class="spticon" :size="32" :iconNo="item.type" :iconType="1"></supporticon>
              <span class="desp">{{item.description}}</span>
            </li>
          </ul>
        </div>
        <div class="annouce" v-if="seller.bulletin">
          <div class="title">
            <span class="line"></span>
            <h6>商家公告</h6>
            <span class="line"></span>
          </div>
          <p>{{seller.bulletin}}</p>
        </div>
      </div>
      <div class="close">
        <i class="icon-close" @click="closeDetail"></i>
      </div>
    </div>
  </transition>
</div>
</template>

<script type="text/javascript">
  import star from '../subcomponents/star/Star'
  import supporticon from '../subcomponents/supporticon/Supporticon'
  export default{
    props: ['seller'],
    data () {
      return {
        detailshow: false
      }
    },
    created () {
      // console.log(this.seller)
    },
    methods: {
      showDetail () {
        this.detailshow = true
      },
      closeDetail () {
        this.detailshow = false
      }
    },
    components: {
      star,
      supporticon
    }
  }
</script>

<style type="text/css" lang="less" scoped>
@import '../../common/css/common.less';
.header{
  position: relative;
  overflow: hidden;
  color: #fff;
  background-color: rgba(7, 17, 27, 0.5);
  .detail-enter, .detail-leave-to{
    opacity: 0;
  }
  .detail-enter-active, .detail-leave-active{
    transition:  all 0.6s ease;
  }
  .content-wrap{
    display: flex;
    padding: 24px 12px 18px 24px;
    .sign{
      width: 64px;
      height: 64px;
      img{
        width: 100%;
      }
    }
    .intro{
      display: inline-block;
      margin-left: 16px;
      padding: 2px 0 2px;
      .title{
        overflow: hidden;
        .brand{
          float: left;
          display: inline-block;
          width: 30px;
          height: 18px;
          .bg-image(brand);
          background-size: 30px 18px;
          background-repeat: no-repeat;
          vertical-align: middle;
        }
        .name{
          float: left;
          overflow: hidden;
          margin-left: 6px;
          font-size: 16px;
          font-weight: bold;
          line-height: 18px;
        }
      }
      .description{
        font-size: 12px;
        line-height: 12px;
        margin-top: 8px;
      }
      .supports{
        height: 12px;
        line-height: 12px;
        margin-top: 10px;
        overflow: hidden;
        .spticon{
          float: left;
        }
        .desp{
          float: left;
          height: 12px;
          line-height: 12px;
          font-size: 12px;
          margin-left: 4px;
        }
      }
    }
  }
  .sptCount{
    position: absolute;
    right: 12px;
    bottom: 41px;
    height: 14px;
    padding: 7px 8px;
    line-height: 14px;
    background: rgba(0,0,0,0.2);
    border-radius: 24px;
    .count{
      float: left;
      vertical-align: middle;
      // line-height: 12px;
      font-size: 12px;
    }
    .icon-keyboard_arrow_right{
      font-size: 10px;
      line-height: 14px;
      margin-left: 2px;
      float: right;
      content: '\e905';
    }
  }
  .bulletin{
    height: 28px;
    background: rgba(7, 17, 27, 0.2);
    line-height: 28px;
    display: flex;
    // align-items: center;
    .bulletbg{
      display: inline-block;
      flex: 0 0 22px;
      width: 22px;
      height: 12px;
      margin: 8px 4px 0 12px;
      background-size: 22px 12px;
      background-repeat: no-repeat;
      .bg-image(bulletin);
    }
    .bullettext{
      height: 28px;
      flex: 1;
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      font-size: 12px;
      line-height: 28px;
      // padding: 9px 0;
      // display: table-cell;
      // vertical-align: middle;
    }
    .icon-keyboard_arrow_right{
      flex: 0 0 16px;
      line-height: 28px;
      margin: 0 12px 0 4px;
      content: '\e905';
    }
  }
  .bg{
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    width: 100%;
    filter: blur(10px);
    z-index: -1;
    img{
      width: 100%;
    }
  }
  .detail{
    position: fixed;
    left: 0;
    top: 0;
    overflow-y: scroll;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    background: rgba(7,17,27,0.8);
    -webkit-backdrop-filter: blur(10px);
    color: #fff;
    z-index: 99;
    .wrap{
      flex: 1;
      padding: 64px 36px 36px;
      h4{
        text-align: center;
        font-size: 16px;
        font-weight: 700;
        line-height: 16px;
      }
      .starWrap{
        margin: 16px auto 28px;
        text-align: center;
        .star{
          display: inline-block;
        }
      }
      .title{
        display: flex;
        .line{
          flex: 1;
          height: 0;
          position: relative;
          top: 7px;
          .border-1px(rgba(255,255,255,0.2));
        }
        h6{
          // flex: auto;
          padding: 0 12px;
          font-size: 14px;
          font-weight: 700;
          line-height: 14px;
        }
      }
      .suptlist{
        margin: 24px 0 0 12px;
        li{
          height: 16px;
          line-height: 16px;
          margin-bottom: 12px;
          .spticon{
            float: left;
            margin-right: 6px;
          }
          .desp{
            float: left;
            font-size: 12px;
          }
        }
      }
      .annouce{
        margin-top: 28px;
        p{
          margin: 24px 12px 0;
          font-size: 12px;
          line-height: 24px;
        }
      }

    }
    .close{
      flex: 0;
      padding-bottom: 32px;
      height: 32px;
      text-align: center;
      .icon-close{
        content: '\e903';
        color: rgba(255,255,255,0.5);
        font-size: 32px;
        line-height: 32px;
      }
    }
  }
}
</style>
