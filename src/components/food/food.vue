<template>
<div class="food" v-show="showFlog" ref="food">
    <div class="food-content">
        <div class="img-header">
            <img :src="food.image">
            <div class="back" @click="hide"><i class="icon-arrow_lift"></i></div>  
        </div>
        <div class="content">
            <p class="title">{{food.name}}</p>
            <div class="desc">
                <span class="sell-count">{{'月售'+food.sellCount+'份'}}</span>
                <span class="rating">{{'好评率'+food.rating}}%</span>
            </div>
            <div class="price">
                <span class="now">{{'￥'+food.price}}</span>
                <span v-show="food.oldPrice" class="old">{{'￥'+food.oldPrice}}</span>
            </div>
        <div class="cart-wrapper">
            <cartcontrol :food="food"></cartcontrol>
        </div>
        <div class="buy" v-show="!food.count || food.count==0" @click="addfrist">加入购物车</div>  
        </div>
        <split v-show="food.info"></split>
        <div class="info" v-show="food.info">
            <p class="title">商品信息</p>
            <p class="text">{{food.info}}</p>
        </div>
        <split></split>
        <div class="rating">
            <p class="title">商品评价</p>
            <ratingsect :selectType="selectType" :onlyContent="onlyContent"
            :desc="desc" :ratings="food.ratings"></ratingsect>
            <div class="rating-wrapper">
               <ul  v-show="food.ratings && food.ratings.length">
                   <li v-for="(rating,index) in food.ratings" v-show="needShow(rating.rateType,rating.text)"
                   :key="index" class="rating-item border-1px">
                       <div class="user">
                           <span class="name">{{rating.username}}</span>
                            <img :src="rating.avatar" class="avatar" width='12' height='12'>
                       </div>
                       <div class="tiem">{{rating.rateTime | formatDate}}</div>
                       <p class="text">
                           <span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}">
                           </span>{{rating.text}}
                       </p>
                   </li>
               </ul>
               <div class="no-rating" v-show="!food.ratings || !food.ratings.length">暂无评价</div>
            </div>
        </div>
    </div>
</div>
</template>
<script>
import BScroll from 'better-scroll'
import cartcontrol from 'components/cartcontrol/cartcontrol'
import Vue from 'vue'
import {formatDate} from 'common/js/data'
import split from 'components/split/split'
import ratingsect from 'components/ratingsect/ratingsect'
// const posttive=0
// const negative=1
const all =2
export default {
    name:'food',
    props:{
        food:{
            type:Object
        }
    },
    filters:{
        formatDate(time){
            let data = new Date(time)
            // 调用格式化函数
            return formatDate(data,'yyyy-MM-dd hh:mm')
        }
    },
    components:{
        cartcontrol,
        split,
        ratingsect
    },
    data(){
        return {
            showFlog:false,
            selectType:all,
            onlyContent:false,
            desc:{
                all:'全部',
                posttive:'推荐',
                negative:'吐槽'
            }
        }
    },
    methods:{
        show(){
            this.showFlog=true
            this.selectType=all
            this.onlyContent=false
            this.$nextTick(() => {
                 if(!this.scroll){
                     this.scroll=new BScroll(this.$refs.food,{
                         click:true
                         })
                 }else{
                     this.scroll.refresh()
                 }
             })
        },
        hide(){
            this.showFlog=false
        },
        addfrist(){
            Vue.set(this.food,'count',1)
        },
        needShow(type,text){
            if(this.onlyContent && !text){
                return false
            }
            if(this.selectType === all){
                return true
            }else{
                return type ===this.selectType
            }
        }
    },
    events:{
        'ratingtype.select'(type){
            this.selectType=type
            thi.$nextTick(() =>{
                this.scroll.refresh()
            })
            
        },
        'content.toggle'(onlyContent){
            this.onlyContent=onlyContent
            this.$nextTick(() =>{
                this.scroll.refresh()
            })
        }
    }
}
</script>

<style lang="stylus" scoped>
@import '~common/stylus/mixin.styl'
.food
    overflow hidden
    position fixed
    top:0
    left:0
    bottom:48px
    z-index:102
    width:100%
    background:#fff
    .img-header
     position relative
     width:100%
     height:0
     padding-top:100%
     img 
      position absolute
      top:0
      left:0
      width:100%
      height:100%
     .back
        position:absolute
        top:10px
        left:0
        .icon-arrow_lift
           display block
           padding  10px
           font-size 20px
           color:#fff
    .content
        position:relative
        padding 18px 
        .title
            font-size 14px
            line-height:14px
            margin-bottom:8px
            font-weight 700
            color:rgb(7,17,27)
        .desc
            margin-bottom 18px
            line-height:10px
            font-size:0
            .sell-count,.rating
              font-size:10px
              color:rgb(147,153,159)
            .sell-count
                margin-right:12px
        .price
          .now
            margin-right 8px 
            font-size:14px
            color #ff0000 
            font-weight 700
            line-height:24px
            .old
                font-size 10px
                text-decoration:line-through
                font-size:10px
                color:rgb(147,153,159)
                font-weight 700
                line-height:24px
        .cart-wrapper
            position:absolute
            right:12px
            bottom:12px
        .buy
            position absolute
            right:18px
            bottom:18px
            z-index:10
            line-height:24px
            padding:0 12px
            box-sizing:border-box
            font-size:10px
            border-radius:12px
            color:#fff
            background:rgb(0,160,220)
    .info
        padding 18px
        .title
            margin-bottom 6px
            font-size 14px
            color:rgb(7,17,27)
        .text
            line-height 24px
            font-size:12px
            color:rgb(77,85,93)
    .rating
      padding  18px
      .title
        line-height:14px
        margin-left 18px
        font-size 14px
        color:rgb(7,17,27)
    .rating-wrapper
        padding 0 18px
        .rating-item
            position relative
            padding:16px 0
            border-1px(rgba(7,17,27,.1))
            .user
                position:absolute
                right:0
                top:16px 
                line-height 12px
                font-size:0
                .name
                    display inline-block
                    vertical-align top
                    margin-right:6px 
                    font-size:10px 
                    color:rgb(147,153,159)
                .avatar
                    border-radius 50%
            .time
               line-height 12px
               font-size 10px
               color:rgb(147,153,159)       
               margin-bottom 6px
            .text
                line-height 16px
                font-size 12px
                color:rgb(7,17,27)
                .icon-thumb_up,.icon-thumb_down 
                    line-height 24px
                    margin-right:4px 
                    font-size:12px 
                .icon-thumb_up
                    color:rgb(0,160,220)
                .icon-thumb_down
                    color:rgb(147,153,159)        
        .no-rating
            padding 16px 0
            font-size 12px
            color:rgb(147,153,159)

                    




</style>
