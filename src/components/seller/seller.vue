<template>
  <div class="seller">
    <div class="seller-content" v-el='seller'>
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc border-1px">
            <star :size="36" :score="seller.score"></star>
            <span class="text">（{{seller.ratingCount}}）</span>
            <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
            <li class="block">
                <h2>起送价</h2>
                <div class="content">
                    <span class="stress">{{seller.minPrice}}</span>元
                </div>
            </li>
              <li class="block">
                <h2>商家配送</h2>
                <div class="content">
                    <span class="stress">{{seller.serviceScore}}</span>元
                </div>
            </li>
              <li class="block">
                <h2>平均配送时间</h2>
                <div class="content">
                    <span class="stress">{{seller.deliveryTime}}</span>分钟
                </div>
            </li>
        </ul>
      </div>
      <div class="bulletin">
          <div class="title">公告与活动</div>
          <div class="content-wrapper border-1px">
              <div class="content">{{seller.bulletin}}</div>
          </div>        
      </div>
       <ul v-if="seller.supports" class="supports">
            <li class="support-item" v-for="item in seller.supports" :key="item.id">
                <span class="icon" :class="classMap[seller.supports[$index].type]"></span>
                <span class="text">{{seller.supports[$index].description}}</span>
            </li>
        </ul>
    </div>
  </div>
</template>
<script>
// import BScroll from "better-scroll"
import star from 'components/star/star'

export default {
    props: {
        seller: {}
    },
    components: {
        star
    },
    // ready() {
    //     this.scroll = new BScroll(this.$els.seller, {
    //         click: true
    //     })
    // },
    created() {
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
}
</script>

<style lang="stylus" scoped>
@import '../../common/stylus/mixin.styl'

.seller
    position absolute
    top 174px
    bottom 0
    left 0
    width 100%
    overflow hidden
    .overview
        padding 18px
        .title
            margin-bottom 8px
            line-height 14px
            color rgb(7,17,27)
            font-size 14px
        .desc
            padding-bottom 18px
            border-1px(raba(7,17,27,0.1))
            font-size 0
            .star
                display inline-block
                margin-right 8px
                vertical-align top
            .text
                display inline-block
                margin-right 12px
                display inline-block
                vertical-align top 
                font-size 10px
        .remark
            display flex
            padding-top 18px
            .block
                flex 1
                text-align center
                border-right 1px solid rgba(7,17,27,0.1)
                &:last-child
                    border:none
                h2
                    margin-bottom 4px
                    line-height 10px
                    font-size 10px
                    color rgb(147,153,159)
                .content
                    line-height 24px
                    font-size 10px
                    color rgb(7,17,27)
                    .stress
                     font-size 24px
    .bulletin
        padding 18px 18px 0 18px
        .title
            margin-bottom 8px
            line-height 14px
            color rgb(7,17,27)
            font-size 14px
        .content-wrapper
            padding 0 12px 16px 12px
            .content
                line-height 24px
                font-size 12px
                color rgb(240,20,20)
                border-1px(raba(7,17,27,0.1))
    .supports
        width 80%
        margin 0 auto
        .support-item
            padding 0 12px
            margin-bottom 12px
            font-size 0
            &:last-child
                margiin-bottom 0
            .icon
                display inline-block
                width 16px
                height 16px
                vertical-align top
                margin-right 6px
                background-size 16px 16px
                background-repeat no-repeat
                &.decrease
                    bg-image('decrease_4')
                &.discount
                    bg-image('discount_4')
                &.guarantee
                    bg-image('guarantee_4')
                &.invoice
                    bg-image('invoice_4')
                &.special
                    bg-image('special_4')
            .text
                font-size 12px
                line-height 16px
                color rgb(7,17,27)

</style>

