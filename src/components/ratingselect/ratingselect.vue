<template>
    <div class="ratingselect">
        <div class="rating-type border-1px">
            <span @click="select(2,$event)" class="block positive" :class="{'active':selectType===2}">{{desc.all}}<span class="count">{{ratings.length}}</span> </span>
            <span @click="select(0,$event)" class="block positive" :class="{'active':selectType===0}">{{desc.positive}}<span class="count">{{positives}}</span></span>
            <span @click="select(1,$event)" class="block negative" :class="{'active':selectType===1}">{{desc.negative}}<span class="count">{{negatives}}</span></span>
        </div>
        <div class="switch" :class="{'on':onlyContent}">
            <span @click="showOnlyContent($event)" class="icon-check_circle"></span>
            <span class="text">只看有内容的评价</span>
        </div>
    </div>
</template>
<script  type="text/ecmascript-6">
// const POSITIVE = 0 // 正向评价
// const NEGATIVE = 1 // 不好的评价
const ALL = 2 // 所有评价
export default {
    props: {
        ratings: {
            type: Array,
            default() {
                return []
            }
        },
        selectType: {
            type: Number,
            default: ALL
        },
        onlyContent: {
            type: Boolean,
            default: false
        },
        desc: {
            type: Object,
            default() {
                return {
                    all: '全部',
                    positive: '满意',
                    negative: '不满意'
                }
            }
        }

    },
    computed: {
        positives() {
            return this.ratings.filter(item => item.rateType === 0).length
        },
        negatives() {
            return this.ratings.filter(item => item.rateType === 1).length
        }
    },
    methods: {
        select(type, event) {
            if (!event._constructed) {
                return;
            }
            this.selectType = type;
            this.$emit('selectedtype', this.selectType, this.onlyContent)
        },
        showOnlyContent(event) {
            if (!event._constructed) {
                return;
            }
            this.onlyContent = !this.onlyContent
            this.$emit('selectedtype', this.selectType, this.onlyContent)
        }
    }
}
</script>
<style lang="stylus" scoped>
@import '../../common/stylus/mixin.styl'
.ratingselect
    .rating-type
        padding 18px 0
        border-1px(rgba(7,17,27,0.1))
        .block
            display inline-block
            padding 8px 12px
            margin-right 8px
            border-radius 2px
            line-height 16px
            font-size 12px
            color rgb(77,85,93)
            font-weight 700
            .count
                font-size 8px
                margin-left 2px
            &.positive
                background-color rgba(0,160,220,0.2)
                &.active
                    background-color rgb(0,160,220)
                    color #fff
            &.negative
                background-color rgba(77,85,93,0.2)
                &.active
                    background-color rgb(77,85,93)
                    color #fff

    .switch
        padding 12px 0
        color rgb(147,153,159)
        line-height 24px
        border-bottom 1px solid rgba(7,17,27,0.1)
        &.on
            .icon-check_circle
                color #00c850
        .icon-check_circle
            font-size 24px
            display inline-block
            vertical-align bottom 
            margin-right 4px
        .text
            font-size 12px


        
</style>