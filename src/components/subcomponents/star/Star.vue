<template>
<div class="star" :class="starSize">
  <span class="starItem" v-for="(item, index) in starClass" :class="item" :key="index"></span>
</div>
</template>
<script type="text/javascript">
  const LENGTH = 5
  const CLS_ON = 'on'
  const CLS_HALF = 'half'
  const CLS_OFF = 'off'
  export default {
    props: {
      score: Number,
      size: Number
    },
    computed: {
      starSize () {
        return `star${this.size}`
      },
      starClass () {
        let classarr = []
        let intpart = Math.floor(this.score)
        let decimal = (Math.floor(this.score * 2)) / 2 - intpart
        for (var i = 0; i < intpart; i++) {
          classarr.push(CLS_ON)
        }
        if (decimal === 0.5) {
          classarr.push(CLS_HALF)
        }
        while (classarr.length < LENGTH) {
          classarr.push(CLS_OFF)
        }
        return classarr
      }
    }
  }
</script>
<style lang="less" scoped>
@import '../../../common/css/common.less';
.star{
  &.star24{
    .starItem{
      width: 12px;
      height: 12px;
      background-size: 12px 12px;
      margin-right: 3px;
    }
    .on{
      .bg-image(star24_on);
    }
    .half{
      .bg-image(star24_half);
    }
    .off{
      .bg-image(star24_off);
    }
  }
  &.star36{
    .starItem{
      width: 18px;
      height: 18px;
      background-size: 18px 18px;
      margin-right: 4px;
    }
    .on{
      .bg-image(star36_on);
    }
    .half{
      .bg-image(star36_half);
    }
    .off{
      .bg-image(star36_off);
    }
  }
  &.star48{
    .starItem{
      width: 24px;
      height: 24px;
      background-size: 24px 24px;
      margin-right: 12px;
    }
    .on{
      .bg-image(star48_on);
    }
    .half{
      .bg-image(star48_half);
    }
    .off{
      .bg-image(star48_off);
    }
  }
  .starItem{
    display: inline-block;
    background-repeat: no-repeat;
    &:last-child{
      margin-right: 0;
    }
  }
}
</style>
