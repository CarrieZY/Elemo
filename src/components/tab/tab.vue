<template>
    <div class="tab border-1px">
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
    display: flex
    width: 100%
    height: 40px
    line-height: 40px
    background:#fff
    border-1px(rgba(7, 17, 27, 0.1))
    z-index 50
    .tab-item
      flex: 1
      text-align: center
      & > a
        display: block
        font-size: 14px
        color: rgb(77, 85, 93)
        &.active
          color: rgb(240, 20, 20)

</style>
