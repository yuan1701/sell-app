<template>
    <div class="shopcar">
        <div class="content">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo">
                        <span class="icon-shopping_cart"></span>
                    </div>
                </div>
                <div class="price">￥0</div>
                <div class="desc">另需配送费￥{{deliveryPrice}}</div>
            </div>
            <div class="content-right">
                <div class="pay">￥{{minPrice}}起配送</div>
            </div>
        </div>
    </div>
</template>
<script  type="text/ecmascript-6">
export default {
    props: {
        deliveryPrice: {
            type: Number,
            default: 0
        },
        minPrice: {
            type: Number,
            default: 0
        }
    }
}
</script>
<style lang="stylus" scoped>
.shopcar
    position fixed
    bottom 0
    left 0
    z-index 50
    height 48px
    width 100%
    background #000000
    .content
        display flex
        font-size 0
        color rgba(255,255,255,0.4)
        .content-left
            flex 1
            .logo-wrapper
                display inline-block
                vertical-align top
                position relative
                top -10px
                margin 0 12px
                padding 6px
                width 56px
                height 56px
                box-sizing border-box
                border-radius 50%
                background #000
                .logo
                    width 100%
                    height 100%
                    border-radius 50%
                    background #2b343c
                    text-align center
                    .icon-shopping_cart
                        font-size 24px
                        line-height 44px
                        color #80858a
            .price
                display inline-block
                vertical-align top
                line-height 24px
                margin-top 12px
                padding-right 12px
                border-right 1px solid rgba(255,255,255,0.1)
                box-sizing border-box
                font-size 16px
                font-weight 700
            .desc
                display inline-block
                margin 12px 0 0 12px
                font-size 10px
                line-height 24px
        .content-right
            flex 0 0 105px
            width 105px
            .pay
                line-height 48px
                height 48px
                text-align center
                font-size 12px
                background #2b333b

            

</style>

