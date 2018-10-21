<template>
    <div class="shopcart">
        <div class="content">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo">
                        <span class="icon-shopping_cart"></span>
                    </div>
                </div>
                <div class="price">0元</div>
                <div class="desc">另需配送费￥元</div>
            </div>
            <div class="content-right">￥20起送</div>
        </div>
    </div>
</template>

<script>
export default {
    name:'shopcart'
}
</script>

<style lang="stylus" scoped>
@import '~common/stylus/mixin.styl'
.shopcart
  position fixed
  z-index:50
  left:0
  bottom:0
  width 100%
  height 48px
  background:#141d27
  font-size 0
  .content
    display flex
    .content-left
      flex 1
      .logo-wrapper
        display inline-block
        position relative
        top:-10px
        margin:0 12px
        padding:6px
        width:56px
        height:56px
        border-radius:50%
        box-sizing: border-box
        vertical-align top
        background:#141d27
        .logo
            height:100%
            width:100%
            text-align center
            border-radius:50%
            background:#2b343c
            .icon-shopping_cart
              font-size:24px 
              color:#80858a
              line-height 44px
      .price
        display inline-block
        vertical-align top
        line-height:24px 
        margin-top:12px 
        padding-right 12px
        border-right:2px solid rgba(255,255,255,.2)
        font-size 16px
        font-weight:700 
        color:rgba(255,255,255,0.5)
      .desc
        display inline-block
        vertical-align top
        line-height 24px
        margin 12px 0 0 12px
        color:rgba(255,255,255,0.5)
        font-size 10px 
    .content-right
      flex 0 0 105px
      width:105px 
      background rgb(37,44,51)
      text-align center
      line-height:48px
      font-weight 700
      font-size:10px 
      color:rgba(255,255,255,0.5)
      
</style>
