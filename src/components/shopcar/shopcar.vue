<template>
    <div class="shopcar">
        <div class="content">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo" @click="showSelectDetails" :class="{'highlight':totalCount>0}">
                        <span class="icon-shopping_cart" :class="{'highlight':totalCount>0}"></span>
                    </div>
                    <div class="num" v-show="totalCount>0">{{totalCount}}</div>
                </div>
                <div class="price" :class="{'highlight':totalPrice>0}">￥{{totalPrice}}</div>
                <div class="desc">另需配送费￥{{deliveryPrice}}</div>
            </div>
            <div class="content-right">
                <div @click="goPay" class="pay" :class="payClass">{{payDesc}} </div>
            </div>
        </div>
        <div class="ball-container">
            <div trasition="drop" v-for="ball in balls" v-show="ball.show" :key="ball.index" class="ball">
                <div class="inner inner-hook"></div>
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
                        // price: 40,
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
    data() {
        return {
            balls: [
                {show: false}, {show: false}, {show: false}, {show: false}, {show: false}
            ],
            dropBall: []
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
        },
        payDesc() {
            if (this.totalPrice === 0) {
                return `${this.minPrice}元起送`;
            } else if (this.totalPrice < this.minPrice) {
                let diff = this.minPrice - this.totalPrice;
                return `还差￥${diff}起送`;
            } else {
                return `去结算`;
            }
        },
        payClass() {
            if (this.totalPrice < this.minPrice) {
                return 'not-enough'
            } else {
                return 'enough'
            }
        }
    },
    methods: {
        // 购物车详情
        showSelectDetails() {
            console.log(this.selectFoods)
        },
        // 去支付
        goPay(price) {
            console.log('共' + this.totalPrice + '元')
        },
        drop(el) {
           for (let i = 0; i < this.balls.length; i++) {
                let ball = this.balls[i];
                if (!ball.show) {
                    ball.show = true;
                    ball.el = el;
                    this.dropBall.push(ball);
                    return;
                }
           }
        }

    },
    transition: {
        drop: {
            beforeEnter(el) {
                let count = this.balls.length;
                while (count--) {
                    let ball = this.balls[count];
                    if (ball.show) {
                        // 用于获取小球相对于视窗的位置集合
                        let rect = ball.el.getBoundingClientrect();
                        let x = rect.left - 32;
                        let y = -(window.innerHeight - rect.top - 22);
                        el.style.display = '';
                        el.style.webKitTransform = `translate3d(0,${y}px,0)`;
                        el.style.transform = `translate3d(0,${y}px,0)`;

                        let inner = el.getElementsByClassName('inner-hook')[0];
                        inner.style.webKitTransform = `translate3d(${x}px,0,0)`;
                        inner.style.transform = `translate3d(${x}px,0,0)`;
                    }
                }
            },
            enter(el) {
                /* eslint-disable no-unused-vars */
                let rf = el.offsetHeight;
                this.$.nextTick(() => {
                    el.style.webKitTransform = 'translate3d(0,0,0)';
                    el.style.transform = 'translate3d(0,0,0)';

                    let inner = el.getElementsByClassName('inner-hook')[0];
                    inner.style.webKitTransform = 'translate3d(0,0,0)';
                    inner.style.transform = 'translate3d(0,0,0)';
                })
            },
            afterEnter(el) {
                let ball = this.dropBall.shift();
                if (ball) {
                    ball.show = false;
                    el.style.display = '';
                }
            }
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
                &.not-enough
                    background #2b333b
                &.enough
                    background #00b43c
                    color #ffffff 
    .ball-container
        .ball
            position absolute
            left 32px
            bottom 32px
            z-index 200
            &.drop-transition 
                transition all 0.4s cubic-bezier(0.49, -0.29, 0.75, 0.41) 
                .inner
                    width 16px 
                    height 16px
                    border-radius 50%
                    color #39f
                    transition all 0.4s linear 
</style>

