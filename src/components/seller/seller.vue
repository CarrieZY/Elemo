<template>
    <div class="seller" ref="seller">
        <div class="seller-content">
            <div class="overview">
                <p class="titile">{{seller.name}}</p>
                <div class="desc border-1px">
                   <star :size="36" :score="seller.score"></star>
                   <span class="text">{{'('+seller.ratingCount+')'}}</span>
                   <span class="text">月售{{seller.sellCount}}单</span>
                </div>
                <ul class="remark">
                    <li class="block">
                        <p>起送价</p>
                        <div class="content">
                            <span class="stress">{{seller.minPrice}}</span>元
                        </div>
                    </li>
                    <li class="block">
                        <p>商家配送</p>
                        <div class="content">
                            <span class="stress">{{seller.deliveryPrice}}</span>元
                        </div>
                    </li>
                    <li class="block">
                        <p>平均配送时间</p>
                        <div class="content">
                            <span class="stress">{{seller.deliveryTime}}</span>分钟
                        </div>
                    </li>
                </ul>
                <div class="favorite" @click="toggleFavorite">
                    <span class="icon-favorite"  :class="{'active':favorite}"></span>
                    <span class="text">{{favoriteText}}</span>
                </div>
            </div>
            <split></split>
            <div class="notice">
                <div class="title">公告与活动</div>
                <div class="content-wrapper border-1px">
                    <div class="bulletin">{{seller.bulletin}}</div>
                </div>
               <ul v-if="seller.supports" class="supports">
                    <li class="support-item border-1px" v-for="(item,index) in seller.supports" :key="index">
                    <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                    <span class="text">{{seller.supports[index].description}}</span>
                    </li>
                </ul>
            </div>
            <split></split>
            <div class="pics">
                <p class="title">商家实景</p>
                <div class="pics-wrapper" ref="picswrapper">
                    <ul class="pics-list" ref="piclist">
                        <li class="pics-item" v-for="(pic,index) in seller.pics" :key="index">
                            <img :src="pic" width="120" height="90">
                        </li>
                    </ul>
                </div>
            </div>
            <split></split>
            <div class="info">
                <div class="title">商家信息</div>
                <ul class="info-wrapper">
                    <li class="info-item" v-for="(info,index) in seller.infos" :key="index">{{info}}</li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import BScroll from 'better-scroll'
import star from 'components/star/star'
import split from 'components/split/split'
export default {
    name:'seller',
    props:{
        seller:{
            type:Object
        }
    },
    data(){
        return {
            favorite:false
        }
    },
    components:{
        star,
        split
    },
    mounted(){
        this.$nextTick(() =>{
            this._initScroll()
            this._initPics()
        })
    },
    watch:{
        'seller'(){
            this.$nextTick( () =>{
                this._initScroll()
                this._initPics()
            })
        }
    },
    created (){
        this.classMap=['decrease','discount','guarantee','invoice','special']
    },
    computed:{
        favoriteText(){
            return this.favorite?'已收藏':'未收藏'
        }   
    },
    methods:{
        toggleFavorite(){
            this.favorite = !this.favorite
        },
        _initScroll(){
            if(!this.scroll){
                this.$nextTick(() =>{
                    this.scroll = new BScroll(this.$refs.seller,{
                        click:true,
                        eventPassthrough:'vertical'
                    })
                })
            }else{
                this.scroll.refresh()
            }
        },
        _initPics(){
            if(this.seller.pics){
                let picWidth = 120 
                let margin = 6
                let width = (picWidth+margin)*this.seller.pics.length-margin
                this.$refs.piclist.style.width=width 
                if(!this.pics){
                    this.pics=new BScroll(this.$refs.picswrapper,{
                        eventPassthrough:'vertical'
                    })
                }else{
                    this.pics.refresh()
                }
            }
        }
    }

}
</script>

<style lang="stylus" scoped>
@import '~common/stylus/mixin.styl'
.seller
    position absolute
    top:178px
    bottom:0
    left:0
    width:100%
    overflow hidden
    .overview
        padding 18px 
        .titile
            margin-bottom:8px 
            font-size 14px 
            line-height 14px 
            color:rgb(7,17,27)
        .desc 
            padding-bottom 18px 
            font-size:0 
            line-height 18px
            border-1px(rgba(7,17,27,.1))
            .star
                display inline-block
                margin-right:8px 
            .text
                display inline-block
                vertical-align top
                font-size 10px 
                color:rgb(77,85,93)
                margin-right 12px 
        .remark
            display flex 
            padding 18px 0
            .block
                flex:1 
                text-align center
                border-right:1px solid rgba(7,17,27,.1)
                &:last-child
                    border-right:none
                p
                  font-size 10px 
                  line-height 10px 
                  margin-bottom 4px 
                  color:rgb(147,153,159)   
                .content
                    font-size 10px 
                    color:rgb(7,17,27)
                    .stress
                        font-size 24px 
                        line-height 24px
        .favorite
            position absolute  
            right:18px 
            top:18px
            text-align center 
            .icon-favorite
                display block
                font-size 24px 
                line-height:24px 
                color:#d4d6d9
                margin-bottom 4px 
                &.active
                  color:rgb(240,20,20)  
            .text 
                display block    
                font-size 10px 
                line-height:10px 
                color:rgb(77,85,93)

    .notice
        padding 18px    
        .title
            margin-bottom:8px 
            font-size 14px 
            line-height 14px 
            color:rgb(7,17,27)
        .content-wrapper
            padding 0 12px 16px 12px
            border-1px(rgba(7,17,27,.1))
            .bulletin
                font-size 12px 
                line-height:24px 
                color:rgb(240,20,20)
        .supports
            .support-item
                padding:16px 12px
                border-1px(rgba(7,17,27,.1))
                &:last-child
                    border-none()
                .icon
                    display inline-block 
                    width:16px 
                    height 16px 
                    margin-right:6px 
                    background-repeat no-replace
                    margin-right:6px 
                    background-size 16px 16px
                    &.decrease
                        bg-image('decrease_2')
                    &.discount
                        bg-image('discount_2')
                    &.guarantee
                        bg-image('guarantee_2')   
                    &.invoice
                        bg-image('invoice_2')
                    &.special
                        bg-image('special_2')  
                .text
                    display inline-block
                    vertical-align top
                    font-size 12px 
                    line-height:16px 
                    color:rgb(7,17,27) 
    .pics
        padding 18px
        .title
          margin-bottom:12px 
          font-size 14px 
          line-height 14px 
          color:rgb(7,17,27)  
        .pics-wrapper
            width:100%
            overflow hidden
            white-space nowrap
            .pics-list
                font-size 0
                .pics-item
                    display inline-block
                    margin-right:6px 
                    width:120px 
                    height:90px 
                    &:last-child
                        margin-right 0
    .info
        padding 18px 
        .title
            margin-bottom:12px 
            font-size 14px 
            line-height 14px 
            color:rgb(7,17,27) 
        .info-item 
            padding 18px 12px
            font-size:12px 
            border-1px(rgba(7,17,27,.1))
            color:rgb(7,17,27)
            &:last-child
                border-none()


</style>
