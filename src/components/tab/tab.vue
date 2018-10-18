<template>
    <div class="tab">
        <router-link tag="a" to="/goods" class="tab-item">商品</router-link>
        <router-link tag="a" to="/ratings" class="tab-item">评价</router-link>
        <router-link tag="a" to="/seller" class="tab-item">商家</router-link>
    </div>
</template>

<script>
export default {
    name:'tab'
}
</script>

<style lang="stylus" scoped>
@import '~common/stylus/mixin.styl'
.tab
    display flex
    width:100%
    height 40px
    line-height 40px
    .tab-item
        flex 1
        text-align center
</style>
