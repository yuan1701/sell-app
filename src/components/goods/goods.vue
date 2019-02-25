<template>
    <div class="goods">
       <div class="menu-wrapper">
           <ul>
               <li v-for="item in goods" :key="item.id">
                   <span class="text">
                       <span class="icon" :class="classMap[item.type]"></span>{{item.name}}
                   </span>
               </li>
           </ul>
        </div> 
       <div class="foods-wrapper"></div> 
    </div>
</template>
<style lang="stylus" scoped>
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
            // .menu-item
        .foods-wrapper
            flex 1
</style>
<script  type="text/ecmascript-6">
const ERR_OK = 0;

export default {
    props() {
        return {
            data: {
                goods: []
            }
        }
    },
    created() {
        this.$http.get("/api/goods").then(response => {
            response = response.body;
            if (response.errno === ERR_OK) {
                this.goods = response.data;
                console.log(this.goods);
            }
        });
        this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
};
</script>


