<template>
    <div class="goods">
        <div class="menu-wrapper" v-el:menu-wrapper >
           <ul>
               <li v-for="item in goods" :key="item.id" class="menu-item" :class="{'current':currentIndex===$index}" @click="selectMenu($index,$event)">
                    <span class="text">   
                        <span class="icon" v-show="item.type>0" :class="classMap[item.type]"></span>
                    {{item.name}} </span>
               </li>
           </ul>
        </div> 
        <div class="foods-wrapper" v-el:food-wrapper>
            <ul>
                <li v-for="item in goods" :key="item.id" class="food-list food-list-hook">
                    <h1 class="title">{{item.name}}</h1>
                    <ul>
                        <li @click="selectFood(food, $event)" v-for="food in item.foods" :key="food.id" class="food-item">
                            <div class="icon">
                                <img :src="food.icon" alt="">
                            </div>
                            <div class="content">
                                <h2 class="name">{{food.name}}</h2>
                                <p class="desc">{{food.description}}</p>
                                <div class="extra">
                                    <span class="count">月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                                </div>
                                <div class="price">
                                    <span class="now">￥{{food.price}}</span>
                                    <span class="old" v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                                </div>
                                <div class="cartcontrol-wrapper">
                                    <cartcontrol :food='food'></cartcontrol>
                                </div>
                            </div>
                        </li>
                    </ul>
                   
                </li>
            </ul>
        </div> 
        <shopcar v-ref:shopcar :select-foods="selectFoods()" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcar>
        <food v-ref:food :food="selectedFood"></food>

    </div>
</template>

<script  type="text/ecmascript-6">
import BScroll from "better-scroll";
import shopcar from "components/shopcar/shopcar";
import cartcontrol from "components/cartcontrol/cartcontrol";
import food from "components/food/food";

const ERR_OK = 0;

export default {
    props: {
        seller: {}
    },
    data() {
        return {
            goods: [],
            listHeight: [],
            scrollY: 0,
            selectedFood: {}
        }
    },
    computed: {
        currentIndex() { // 当前左侧索引位置
            for (let i = 0; i < this.listHeight.length; i++) {
                let height1 = this.listHeight[i];
                let height2 = this.listHeight[i + 1];
                if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
                    return i;
                }
            }
            return;
        }
    },
    created() {
        const dev = process.env.NODE_ENV === "development"
        dev && this.$http.get("/api/goods").then(response => {
            response = response.body;
            if (response.errno === ERR_OK) {
                this.goods = response.data;
                this.$nextTick(() => { // 保证dom已经渲染好了
                    this._initScroll();
                    this._calculateHeight();
                })
            }
        });
        if (!dev) {
            this.goods = require('../../../data.json').goods
            this.$nextTick(() => { // 保证dom已经渲染好了
                this._initScroll();
                this._calculateHeight();
            })
        }
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    methods: {
        selectFood(data, event) {
            if (!event._constructed) { // event._constructed默认派发事件为true,PC端没有这个属性--处理PC端会点击两次问题
                return;
            }
            this.selectedFood = data
            this.$refs.food.show()
            console.log(data)
        },
        selectMenu(index, event) {
            if (!event._constructed) { // event._constructed默认派发事件为true,PC端没有这个属性--处理PC端会点击两次问题
                return;
            }
            // v-el访问dom
            let foodList = this.$els.foodWrapper.getElementsByClassName("food-list-hook");
            let el = foodList[index]
            this.foodScroll.scrollToElement(el, 300)
        },
        _drop(target) {
            // v-ref访问子组件
            this.$refs.shopcar.drop(target)
        },
        _initScroll() {
            // 添加滚动效果
            this.menuScroll = new BScroll(this.$els.menuWrapper, {
                click: true // 点击
            });
            this.foodScroll = new BScroll(this.$els.foodWrapper, {
                click: true, // 点击
                probeType: 3 // 实时滚动位置
            });
            this.foodScroll.on('scroll', (pos) => {
                this.scrollY = Math.abs(Math.round(pos.y))
            })
        },
        _calculateHeight() { // 计算食物列表高度
            let foodList = this.$els.foodWrapper.getElementsByClassName("food-list-hook"); // 获得所有食物列表的数组
            let height = 0;
            this.listHeight.push(height);
            for (let i = 0; i < foodList.length; i++) {
                let item = foodList[i];
                height += item.clientHeight; // (内容)可见区域高累加
                this.listHeight.push(height);
            }
        },
        selectFoods() {
            let foods = [];
            this.goods.forEach(good => {
                good.foods.forEach(food => {
                    if (food.count) {
                        foods.push(food)
                    }
                })
            });
            return foods;
        }
    },
    components: {
        shopcar,
        cartcontrol,
        food
    },
    events: {
        'cart.add'(target) {
            this._drop(target);
        }
    }
};
</script>

<style lang="stylus" scoped>
@import "../../common/stylus/mixin";

    .goods
        display flex
        position absolute
        top 174px
        bottom 46px
        width 100%
        overflow hidden
        .menu-wrapper
            flex 0 0 80px
            width 80px
            background #f3f5f7
            .menu-item
                display table
                width 56x
                height 54px
                line-height 14px
                padding 0 12px
                &.current
                    position relative
                    z-index 10
                    margin-top -1px
                    background #ffffff
                    font-weight 700
                    .text
                        border-none()
                .icon
                    display inline-block
                    width 12px
                    height 12px
                    vertical-align top
                    margin-right 2px
                    background-size 12px 12px
                    background-repeat no-repeat
                    &.decrease
                        bg-image('decrease_3')
                    &.discount
                        bg-image('discount_3')
                    &.guarantee
                        bg-image('guarantee_3')
                    &.invoice
                        bg-image('invoice_3')
                    &.special
                        bg-image('special_3')
                .text
                    display table-cell
                    width 56px
                    vertical-align middle
                    font-size 12px
                    border-1px(rgba(7, 17, 27, 0.1))
        .foods-wrapper
            flex 1
            .title
                padding-left 14px
                height 26px
                line-height 26px
                border-left 2px solid #d9dde1
                color rgb(147,153,159)
                font-size 12px
                background #f3f5f7
            .food-item
                display flex
                margin 18px
                padding-bottom 18px
                border-1px(rgba(7, 17, 27, 0.1))
                &:last-child
                    border-none()
                    margin-bottom 0
                .icon
                    flex 0 0 57px
                    margin-right 10px
                    img
                        max-width 100px
                        max-height 100px
                        border-radius 4px
                .content
                    flex 1
                    .name
                        margin 2px 0 8px 0
                        height 14px
                        line-height 14px
                        font-size 14px
                        color rgb(7,17,27)
                    .desc, .extra
                        line-height 10px
                        font-size 10px
                        color rgb(147,153,159)
                    .desc
                        line-height 12px
                        margin-bottom 8px
                    .extra
                        .count
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
                    .cartcontrol-wrapper
                        position absolute
                        right 0
                        bottom 12px

</style>

