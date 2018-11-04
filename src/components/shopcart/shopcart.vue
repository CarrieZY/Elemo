<template>
    <div class="shopcart">
        <div class="content">
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
            <div class="content-right">
                <div class="pay" :class="payClass">
                    {{payDesc}}
                </div>
            </div>
        </div>
        <div class="shopcart-list" v-show="listshow">
            <div class="list-header">
                <p class="title">购物车</p>
                <span class="empty">清空</span>
            </div>
            <div class="listcontent">
                <ul>
                    <li v-for="(food,index) in setctfood" class="food" :key="index">
                        <span class="name">{{food.name}}</span>
                        <div class="price">
                            <span>{{'￥'+food.price*food.count}}</span>
                        </div>
                        <div class="cartcontrol-wrapper">
                            <cartcontrol></cartcontrol>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import cartcontrol from 'components/cartcontrol/cartcontrol'
export default {
    name:'shopcart',
    components:{
        cartcontrol
    },
    data(){
        return{
            listshow:true
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
    }
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
        

      
</style>
