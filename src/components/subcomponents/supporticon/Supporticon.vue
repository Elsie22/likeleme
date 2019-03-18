<template>
<div class="supporticon" :class="iconSize">
  <span class="icon" :class="iconClass"></span>
</div>
</template>
<script type="text/javascript">
  export default {
    props: {
      iconType: Number, // 不同显示的图标样式，_后面的数字
      size: Number, // 尺寸
      iconNo: Number // 编号，名称对应的返回的type类型
    },
    data () {
      return {
        sptClass: ['decrease', 'discount', 'special', 'invoice', 'guarantee']
      }
    },
    computed: {
      iconSize () {
        return `icon${this.size}`
      },
      iconClass () {
        return `${this.sptClass[this.iconNo]}_${this.iconType}`
      }
    }
  }
</script>
<style lang="less" scoped>
@import '../../../common/css/common.less';
.supporticon{
  .icon{
    display: block;
    background-repeat: no-repeat;
  }
  &.icon24{
    .icon{
      width: 12px;
      height: 12px;
      background-size: 12px 12px;
    }
  }
  &.icon32{
    .icon{
      width: 16px;
      height: 16px;
      background-size: 16px 16px;
    }
  }
  .decrease_1{
    .bg-image(decrease_1);
  }
  .discount_1{
    .bg-image(discount_1);
  }
  .special_1{
    .bg-image(special_1);
  }
  .invoice_1{
    .bg-image(invoice_1);
  }
  .guarantee_1{
    .bg-image(guarantee_1);
  }
  .decrease_2{
    .bg-image(decrease_2);
  }
  .discount_2{
    .bg-image(discount_2);
  }
  .special_2{
    .bg-image(special_2);
  }
  .invoice_2{
    .bg-image(invoice_2);
  }
  .guarantee_2{
    .bg-image(guarantee_2);
  }
  .decrease_3{
    .bg-image(decrease_3);
  }
  .discount_3{
    .bg-image(discount_3);
  }
  .special_3{
    .bg-image(special_3);
  }
  .invoice_3{
    .bg-image(invoice_3);
  }
  .guarantee_3{
    .bg-image(guarantee_3);
  }
  .decrease_4{
    .bg-image(decrease_4);
  }
  .discount_4{
    .bg-image(discount_4);
  }
  .special_4{
    .bg-image(special_4);
  }
  .invoice_4{
    .bg-image(invoice_4);
  }
  .guarantee_4{
    .bg-image(guarantee_4);
  }
}
</style>
