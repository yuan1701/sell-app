<template>
    <div class="food" v-show="showflag" transition='move' v-el:food>
        <div class="food-content">
            <div class="image-header">
                <img :src='food.image' alt="">
                <div class="back" @click="hide"><i class="icon-arrow_lift"></i></div>
            </div>
            <div class="content">
                <h1 class="title">{{food.name}}</h1>
                <div class="detail">
                    <span class="sell-count">月售{{food.sellCount}}份</span>
                    <span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                    <span class="now">￥{{food.price}}</span>
                    <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>

                </div>
               
            </div>
            <split v-show="food.info"></split>
            <div class="info" v-show="food.info">
                <h1 class="title">商品介绍</h1>
                <p class="text">{{food.info}}</p>
            </div>
            <split></split>
            <div class="rating">
                <h1 class="title">商品评价</h1>
                <ratingselect :ratings='food.ratings' :select-type='selectType' :only-content='onlyContent' :desc='desc' @selectedtype='changeType'></ratingselect>
            </div>
            <div class="rating-wrapper">
                <ul v-show="food.ratings && food.ratings.length">
                    <li v-show="needShow(item.rateType,item.text)" v-for="item in food.ratings" :key="item" class="rating-item">
                        <div class="user_time">
                            <div class="time">{{item.rateTime | formateDate}}</div>
                            <div class="user">
                                <span class="name">{{item.username}}</span>
                                <img class="avatar" :src="item.avatar" width="12" height="12">
                            </div>
                        </div>
                        
                        <p class="text"> <span :class="{'icon-thumb_up':item.rateType==0,'icon-thumb_down':item.rateType==1}"></span>{{item.text}}</p>
                    </li>
                </ul>
                <div class="no-rating" v-show="!food.ratings || !food.ratings.length">
                    暂无评价
                </div>
            </div>
        </div>
    </div>
</template>
<script type="text/ecmascript-6">
import BScroll from "better-scroll";
import split from "../split/split";
import ratingselect from '../ratingselect/ratingselect'
import { formatDate } from '../../common/js/date.js'

// const POSITIVE = 0 // 正向评价
// const NEGATIVE = 1 // 不好的评价
const ALL = 2 // 所有评价

export default {
    name: 'food',
    components: { split, ratingselect },
    props: {
        food: {
            type: Object
        }
    },
    filters: {
        formateDate(time) {
            const date = new Date(time)
            return formatDate(date, 'yyyy-MM-dd hh:mm')
        }
    },
    data() {
        return {
            showflag: false,
            selectType: ALL,
            onlyContent: false,
            desc: {
                all: '全部',
                positive: '满意',
                negative: '不满意'
            }
        }
    },
    methods: {
        changeType(type, onlyContent) {
            this.selectType = type
            this.onlyContent = onlyContent
             this.$nextTick(() => { // 保证dom已经渲染好了
                this.scroll.refresh()
            });
        },
        needShow(type, text) {
            if (this.onlyContent && !text) {
                return false
            }
            if (this.selectType === ALL) {
                return true
            } else {
                return this.selectType === type
            }
        },
        show() {
            this.showflag = true
            this.selectType = ALL
            this.onlyContent = false
            this.$nextTick(() => { // 保证dom已经渲染好了
                if (!this.scroll) {
                    this.scroll = new BScroll(this.$els.food, {
                        click: true // 点击
                    });
                } else {
                    this.scroll.refresh()
                }
            });
        },
        hide() {
            this.showflag = false
        }
    }
}
</script>
<style lang="stylus" scoped>
@import '../../common/stylus/mixin.styl'
.food
    position fixed
    left 0
    top 0
    bottom 48px
    z-index 30
    width 100%
    background #fff
    &.move-transition
        transition all 0.2s linear
        transform translate3d(0,0,0)
        &.move-enter, &.move-leave
            transform translate3d(100%,0,0)
    .image-header
        position relative
        width 100%
        height 0
        padding-top 100%
        img
            position absolute
            left 0
            top 0
            width 100%
            height 100%
        .back
            position absolute
            top 10px
            left 0
            .icon-arrow_lift
                display block
                padding 18px
                font-size 20px
                color #fff
    .content
        position relative
        padding 18px
        .title
            line-height 14px
            font-size 14px
            margin-bottom 8px
            font-weight 700
            color rgb(7, 17,27)
        .detail
            margin-bottom 18px
            line-height 10px
            font-size 0
            .sell-count, .rating
                font-size 10px
                color rgb(147, 153, 159)
            .sell-count
                margin-right 12px
        .price
            font-weight 100px
            line-height 24px
            .now
                margin-right 8px
                font-size 14px
                color rgb(240,20,20)
            .old
                text-decoration line-through
                font-size 10px
                color rgb(147,153,159)
    .info
        padding 18px
        .title
            line-height 14px
            font-size 14px
            margin-bottom 8px
            font-weight 700
            color rgb(7, 17,27)
        .text
            font-size 12px
            font-weight 200
            line-height 24px
            color rgb(77,85,93, )
    .rating
        padding 18px
        .title
            line-height 14px
            font-size 14px
            margin-bottom 8px
            font-weight 700
            color rgb(7,17,27)
    .rating-wrapper
        padding 0 18px
        .rating-item
            position relative
            padding 16px 0
            border-1px(rgba(7, 17, 27, 0.1))
            .user_time
                display flex
                align-items center
                justify-content space-between
                color rgb(147,153,159)
                line-height 12px
                font-size 10px
                margin-bottom 6px
                .user
                    .name
                        margin-right 6px
                        display inline-block
                        vertical-align top
                    .avatar
                        border-radius 50%
            .text
                line-height 16px
                font-size 12px
                color rgb(7, 17, 27)
                .icon-thumb_down, .icon-thumb_up
                    margin-right 4px
                    font-size 12px
                    line-height 12px
                .icon-thumb_down
                    color rgb(143,153,159)
                .icon-thumb_up
                    color rgb(0,160,220)
    .no-rating
        color rgb(147,153, 159)


</style>


