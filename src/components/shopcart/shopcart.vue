<template>
<div>
    <div class="shopcart">
        <div class="content" @click="togglelist">
            <div class="content-left">
                <div class="logo-wrapper">
                    <div class="logo" :class="{'logo-hightlingth':totalCount>0}">
                        <span class="icon-shopping_cart" :class="{'logo-hightlingth':totalCount>0}"></span>
                    </div>
                    <div class="num" v-show="totalCount>0">{{totalCount}}</div>
                </div>
                <div class="price" :class="{'heiglingth':totalPrice>0}">￥{{totalPrice}}元</div>
                <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
            </div>
            <div class="content-right" @click.stop="pay">
                <div class="pay" :class="payClass">
                    {{payDesc}}
                </div>
            </div>
        </div>
        <transition name="fold">
        <div class="shopcart-list" v-show="listshow">
            <div class="list-header">
                <p class="title">购物车</p>
                <span class="empty" @click="emptyCart">清空</span>
            </div>
            <div class="listcontent" ref="listcontent">
                <ul>
                    <li v-for="(food,index) in setctfood" class="food" :key="index">
                        <span class="name">{{food.name}}</span>
                        <div class="price">
                            <span>{{'￥'+food.price*food.count}}</span>
                        </div>
                        <div class="cartcontrol-wrapper">
                            <cartcontrol :food="food"></cartcontrol>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
        </transition>
    </div>
    <transition name="fade">
    <div class="list-mask" v-show="listshow" @click="hidelist"></div>
    </transition>
</div>
</template>

<script>
import BScroll from 'better-scroll'
import cartcontrol from 'components/cartcontrol/cartcontrol'
export default {
    name:'shopcart',
    components:{
        cartcontrol
    },
    data(){
        return {
           listshow:false
        }
    },
    props:{
        setctfood:{
            type:Array,
            default (){
                return [] 
            }
        },
        deliveryPrice :{
            type:Number,
            default:0
        },
        minPrice:{
            type:Number,
            default:0
        }
    },
    computed:{
        // 计算购物车价格
        totalPrice(){
            let tolal=0
            this.setctfood.forEach((food) => {
                tolal+=food.price*food.count
            })
            return tolal
        },
        // 计算购物车的商品的个数
        totalCount(){
            let count =0
            this.setctfood.forEach((food) =>{
                count+=food.count
            })
            return count
        },
        //计算满多少起送
        payDesc(){
            if(this.totalPrice===0){
                return `￥${this.minPrice}起送`
            }else if(this.totalPrice<this.minPrice){
                let diff=this.minPrice-this.totalPrice
                console.log(diff)
                return `还差${diff}元起送`
            }else{
                return '去结算'
            }
        },
        //计算当前的价格是否满足配送的。
        payClass(){
            if(this.totalPrice<this.minPrice){
                return 'not-enough'
            }else{
                return 'enough'
            }
        }
    },
    methods:{
        togglelist(){
            if(!this.totalCount){
                return 
            }
            this.listshow=!this.listshow
            if(this.listshow){
                this.$nextTick(() =>{
                    this.scroll = new BScroll(this.$refs.listcontent)
                })
            }
        },
        emptyCart(){
            this.setctfood.forEach((food) =>{
                food.count=0
                this.listshow=false
            })
        },
        hidelist(){
            this.listshow=false
        },
        // 结算
        pay(){
            if(this.totalPrice<this.minPrice){
                return
            }
        }
    }
}
</script>

<style lang="stylus" scoped>
@import '~common/stylus/mixin.styl'
.shopcart
  position fixed
  z-index:103
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
            &.logo-hightlingth
                background rgb(0,160,220)
            .icon-shopping_cart
              font-size:24px 
              color:#80858a
              line-height 44px
              &.logo-hightlingth
                  color:#fff
        .num
            position:absolute
            top:0
            right:0
            font-size:10px
            font-weight 700
            color:#fff
            line-height:16px
            background:rgb(240,20,20)
            box-shadow 0 2px 4px 0 rgba(0,0,0,.5);
            width:24px 
            height 16px
            border-radius 6px
            text-align center
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
        &.heiglingth
            color:#fff
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
      .pay
        text-align center
        line-height:48px
        font-weight 700
        font-size:10px 
        color:rgba(255,255,255,0.5)
        &.enough
         background #2b33b2
        &.not-enough
         background:#00b43c 
         color:#fff
        
  .shopcart-list
    position:absolute
    bottom:48px
    left:0
    z-index:-1
    width:100%
    &.fold-enter-active,&.fold-leave-active
        transition all 0.5s
    &.fold-enter,&.fold-leave-to
        transform translate3d(0,0,0)
    .list-header
        height 40px
        background:#f3f5f7
        line-height 40px
        padding  0 18px
        border-bottom 1px  solid #777
        .title
           font-size 14px
           float:left
           color:rgb(7,17,27)
        .empty
            float:right
            color:rgb(0,160,220)
            font-size:12px
    .listcontent
        padding 0 18px
        max-height:217px
        background:#fff
        overflow hidden
        .food
            padding:12px 0
            position:relative
            box-sizing border-box
            border-1px(rgba(7,17,27,.2))
            .name
                font-size:14px
                line-height 24px
                color:rgb(7,17,27)
            .price
                position:absolute
                right:90px
                line-height:24px
                bottom 12px
                font-size:14px 
                color:red
                font-weight:700    
            .cartcontrol-wrapper
                position absolute
                right:0
                bottom:6px

.list-mask
    position fixed
    top:0
    left:0
    background rgba(7,17,27,0.6)
    width:100%
    height:100%
    z-index:40
    &.fade-enter-active,&.fold-enter-leave
        opacity 1
        background rgba(7,17,27,0.6)
    &.fade-enter,&.fade-leave
        opacity 0
        background rgb(7,17,27)
</style>
