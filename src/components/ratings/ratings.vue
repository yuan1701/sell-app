<template>
    <div class="ratings" v-el:ratings>
        <div class="ratings-content" >
            <div class="overview">
                <div class="overview-left">
                    <h1 class="score">{{seller.score}}</h1>
                    <div class="title">综合评分</div>
                    <div class="rank">高于周边商家{{seller.rankRate}}</div>
                </div>
                <div class="overview-right">
                    <div class="score-wrapper">
                        <span class="title">服务态度</span>
                        <star :size="36" :score="seller.serviceScore"></star>
                        <span class="score">{{seller.serviceScore}}</span>

                    </div>
                    <div class="score-wrapper">
                        <span class="title">商品评分</span>
                        <star :size="36" :score="seller.foodScore"></star>
                        <span class="score">{{seller.foodScore}}</span>

                    </div>
                    <div class="delivery-wrapper">
                        <span class="title">送达时间</span>
                        <span class="delivery">{{seller.deliveryTime}}分钟</span>
                    </div>
                </div>
            </div>
            <split></split>
            <ratingselect :ratings='ratings' :select-type='selectType' :only-content='onlyContent' :desc='desc' @selectedtype='changeType'></ratingselect>
            <div class="rating-wrapper">
                <ul v-show="ratings && ratings.length">
                    <li v-show="needShow(item.rateType,item.text)" v-for="item in ratings" :key="item" class="rating-item">
                        <div class="avatar">
                            <img :src="item.avatar" width="16" height="16">
                        </div>
                        <div class="content">
                            <h1 class="name">{{item.username}}</h1>
                            <div class="star-wrapper">
                                <star :size="36" :score="seller.serviceScore"></star>
                                <span class="delivery" v-show="item.deliveryTime">{{item.deliveryTime}}分钟送达</span>
                            </div>

                            <p class="text"> {{item.text}}</p>
                            <div class="recommend" v-show="item.recommend&&item.recommend.length">
                                <span class='icon-thumb_up'></span>
                                <span class="item" v-for="item in item.recommend" :key="item">{{item}}</span>
                            </div>
                            <div class="time">{{item.rateTime | formateDate}}</div>

                        </div>
                        
                    </li>
                </ul>
                <div class="no-rating" v-show="!ratings || !ratings.length">
                    暂无评价
                </div>
            </div>
        </div>
        
    </div>
</template>
<script>
import star from 'components/star/star'
import split from "../split/split"
import BScroll from "better-scroll";
import ratingselect from '../ratingselect/ratingselect'
import { formatDate } from '../../common/js/date.js'

const ALL = 2 // 所有评价
// const ERR_OK = 0;

export default {
    props: {
        seller: {
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
            ratings: [],
            selectType: ALL,
            onlyContent: false,
            desc: {
                all: '全部',
                positive: '满意',
                negative: '不满意'
            }
        }
    },
    created() {
        // const dev = process.env.NODE_ENV === "development"
        // dev && this.$http.get("/api/ratings").then(response => {
        //     response = response.body
        //     if (response.errno === ERR_OK) {
        //         this.ratings = response.data
        //     }
        //     this.$nextTick(()=>{
        //         this.scroll = new BScroll(this.$els.ratings, {
        //                 click: true // 点击
        //             })
        //     })
        // })
        this.ratings = require('../../../data.json').ratings
        this.$nextTick(() => {
            this.scroll = new BScroll(this.$els.ratings, {
                    click: true // 点击
                })
        })
    },
    methods: {
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
        changeType(type, onlyContent) {
            this.selectType = type
            this.onlyContent = onlyContent
             this.$nextTick(() => { // 保证dom已经渲染好了
                this.scroll.refresh()
            });
        }
    },
    components: {
        star,
        split,
        ratingselect
    }
}
</script>
<style lang="stylus" scoped>
@import '../../common/stylus/mixin.styl'

.ratings
    position absolute
    top 174px
    bottom 0
    left 0
    width 100%
    overflow hidden
    .overview
        display flex
        padding 18px
        .title
            margin-bottom 8px
            line-height 14px
            color rgb(7,17,27)
            font-size 14px
        .overview-left
            flex 0 0 137px
            width 137px
            padding 6px 0
            text-align center
            border-right 1px solid rgba(7,17,27,0.1)
            @media only screen and (max-width: 320px)
                flex 0 0 83px
                width 83px
            .score
                margin-bottom 8px
                font-size 24px
                color rgb(255,153,0)
                line-height 28px
            .title
                margin-bottom 8px
                font-size 12px
                line-height 12px
                color rgb(7,17,27)
            .rank
                font-size 10px
                line-height 12px
                color rgb(7,17,27)
        .overview-right
            float 1
            padding 6px 0 6px 24px
            @media only screen and (max-width: 320px)
                padding-left 6px
            .score-wrapper
                margin-bottom 8px
                font-size 0
                .title
                    line-height 18px
                    font-size 12px
                    color rgb(7,17,27)
                .star
                    display inline-block
                    margin 0 12px
                    vertical-align top
                .score
                    font-size 12px
                    line-height 18px
                    color rgb(255,153,0)
            .delivery-wrapper
                font-size 0px
                .title
                    font-size 12px
                    color rgb(7,17,27)
                .delivery
                    margin-left 12px
                    font-size 12px
                    color tba(147,153,159)
    .ratingselect
        padding 0 18px
    .rating-wrapper
        padding 0 18px
        .rating-item
            padding 18px 0
            display flex
            border-1px(rgba(7, 17, 27, 0.1))
            .avatar
                flex 0 0 28px
                width 28px
                margin-right 12px
                img 
                    border-radius 50%
            .content
                position relative
                flex 1
                .name
                    margin-bottom 4px
                    line-height 12px
                    font-size 10px
                .star-wrapper
                    display inline-block
                    margin-bottom 6px
                    font-size 0
                    .star
                        display inline-block
                        margin-right 6px
                        vertical-align top
                .delivery
                    display inline-block
                    vertical-align top
                    font-size 10px
                .text
                    font-size 12px
                    line-height 18px
                    color rgb(7,17,27)
                    margin-bottom 8px
                .recommend
                    line-height 16px
                    .icon-thumb_up, .item
                        display inline-block
                        margin 0 8px 4px 0
                        font-size 9px
                    .icon-thumb_up  
                        color rgb(0,160,220)
                    .item
                        padding 0 6px
                        border 1px solid rgba(7,17,27 0.1)
                        border-radius 1px
                        color rgb(147,153,159)
                        line-height 18px
                .time
                    position absolute
                    right 0
                    top 0
                    font-size 10px
</style>


