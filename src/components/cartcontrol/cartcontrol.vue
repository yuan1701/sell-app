<template>
    <div class="cartcontrol">
        <div class="cart-decrease icon-remove_circle_outline" v-show="food.count>0" @click="decreaseCart($event)"></div>
        <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
        <div class="cart-add icon-add_circle" @click="addCart($event)"></div>

    </div>
</template>
<script type="text/ecmascript-6">
import vue from 'vue';
export default {
    props: {
        food: {
            type: Object
        }
    },
    methods: {
        addCart(event) {
            if (!event._constructed) {
                return;
            }
            if (!this.food.count) {
                vue.set(this.food, 'count', 1);
            } else {
                this.food.count++;
            }
        },
        decreaseCart(event) {
            if (!event._constructed) {
                return;
            }
            if (this.food.count) {
                this.food.count--;
            }
        }
    }
}
</script>
<style lang="stylus" scoped>
.cartcontrol
    font-size 0
    .cart-decrease, .cart-add
        display inline-block
        padding 6px
        line-height 24px
        font-size 24px
        color #39f
    .cart-count
        display inline-block
        vertical-align top
        padding-top  6px
        line-height 24px
        text-align center
        font-size 10px
        color rgb(147,153,159)
    .cart-add
        display inline-block
        


</style>

