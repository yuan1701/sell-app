<template>
    <div class="shopcar">
        <div class="content">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo" :class="{'highlight':totalCount>0}">
                        <span class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></span>
                    </div>
                    <div class="num" v-show="totalCount>0">{{totalCount}}</div>
                </div>
                <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
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
        selectFoods: {
            type: Array,
            default() {
                return [
                    {
                        // price: 9,
                        // count: 1
                    }
                ];
            }
        },
        deliveryPrice: {
            type: Number,
            default: 0
        },
        minPrice: {
            type: Number,
            default: 0
        }
    },
    computed: {
        totalPrice() {
            let total = 0;
            this.selectFoods.forEach((food) => {
                total += food.price * food.count;
            });
            return total;
        },
        totalCount() {
            let count = 0;
            this.selectFoods.forEach((food) => {
                count += food.count;
            });
            return count;
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
                    &.highlight
                        background rgb(0,160,220)
                    
                    .icon-shopping_cart
                        font-size 24px
                        line-height 44px
                        color #80858a
                        &.highlight
                            color #fff
                        
                .num 
                    position absolute
                    top 0
                    right 0
                    width 25px
                    height 16px
                    line-height 16px
                    text-align center
                    border-radius 16px
                    font-size 9px
                    font-weight 700
                    background rgb(240,20,20)
                    color #fff
                    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4)
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
                &.highlight
                    color #fff
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

