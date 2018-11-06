<template>
    <div class="ratings" ref="ratings">
        <div class="ratings-content">
            <div class="overview">
                <div class="overview-left">
                    <h1 class="score">{{seller.score}}</h1>
                    <div class="title">综合评分</div>
                    <div class="rank">高于周边商家{{seller.rankRate}}%</div>
                </div>
                <div class="overview-right">
                    <div class="score-wrapper">
                        <span class="title">服务态度</span>
                        <star :size="36" :score="seller.serviceScore"></star>
                        <span class="score">{{seller.serviceScore}}</span>
                    </div>
                    <div class="score-wrapper">
                        <span class="title">商家评分</span>
                        <star :size="36" :score="seller.serviceScore"></star>
                        <span class="score">{{seller.foodScore}}</span>
                    </div>
                     <div class="score-wrapper">
                        <span class="title">送达时间</span>
                        <span class="time">{{seller.deliveryTime}}分钟</span>
                    </div>
                </div>
            </div>
            <split></split>
            <ratingsect :selectType="selectType" :onlyContent="onlyContent" :ratings="ratings"></ratingsect>
            <div class="rating-wrapper">
                <ul>
                    <li v-for="(rating,index) in ratings" :key="index" class="rating-item border-1px">
                        <div class="avatar">
                            <img :src="rating.avatar" width="28" height="28">
                        </div>
                        <div class="content">
                            <p class="name">{{rating.username}}</p>
                            <div class="star-wrapper">
                               <star :size="24" :score="rating.score"></star>
                               <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}分钟送达</span>
                            </div>
                            <p class="text">{{rating.text}}</p>
                            <div class="recomend" v-show="rating.recommend">
                                <span class="icon-thumb_up"></span>
                                <span  class="item" v-for="(item,index) in rating.recommend" :key="index">
                                    {{item}}
                                </span>
                            </div>
                            <div class="time">{{rating.rateTime | formatDate}}</div>
                        </div>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import star from 'components/star/star'
import split from 'components/split/split'
import ratingsect from 'components/ratingsect/ratingsect'
import BScroll from 'better-scroll'
import axios from 'axios'
import {formatDate} from 'common/js/data'
const all =2
export default {
    name:'ratings' ,
    props:{
        seller:{
            type:Object
        }
    },
    data(){
        return {
            ratings:[],
            selectType:all,
            onlyContent:true
        }
    },
    filters:{
        formatDate(time){
            let data = new Date(time)
            // 调用格式化函数
            return formatDate(data,'yyyy-MM-dd hh:mm')
        }
    },
    created(){
        axios.get('static/data.json').then( (res) =>{
            console.log(res)
            this.ratings=res.data.ratings
            console.log(this.ratings)
            this.$nextTick(() =>{
                this.scroll =new BScroll(this.$refs.ratings,{
                    click:true
                })
            })
           
         })

    },
    components:{
        star,
        split,
        ratingsect
    }   
}
</script>

<style lang="stylus" scoped>
@import '~common/stylus/mixin.styl'
.ratings
    position absolute
    top:178px
    bottom:0
    left:0
    width:100%
    overflow:hidden
    .ratings-content
        .overview
            display flex 
            padding 18px 0 
            .overview-left
                flex:0 0 137px
                width:137px 
                border-right 1px solid rgba(7,17,27,.1)
                text-align center
                @media (max-width:320px)
                    flex:0 0 120px
                    width:120px 
                .score
                    margin-bottom 6px
                    font-size 24px 
                    line-height:28px
                    color:rgb(255,153,0)
                .title
                    margin-bottom 8px 
                    line-height:16px
                    font-size 16px 
                    color:rgb(7,17,27) 
                .rank
                    font-size:10px  
                    line-height:10px 
                    color:rgb(147,153,159)
                    padding-bottom 6px  

            .overview-right
                flex:1
                padding-left:24px
                @media (max-width:320px)
                    padding-left 6px
                .score-wrapper
                    margin-bottom 8px
                    line-height:18px 
                    font-size 0 
                    .title
                        display inline-block
                        line-height:16px
                        font-size 12px 
                        color:rgb(7,17,27)
                    .star
                        display inline-block
                        padding 0 12px
                    .score 
                        display inline-block
                        vertical-align top
                        font-size 12px 
                        line-height:18px 
                        color:rgb(255,153,0)
                    .time
                        margin-left:12px 
                        font-size:12px
                        line-height 18px 
                        color:rgb(147,153,159)    
        .rating-wrapper
            padding 0 18px
            .rating-item
                display flex
                padding:18px 0
                border-1px(rgba(7,17,27,.1))
                .avatar
                    margin-right 12px 
                    flex:0  0 28px
                    width:28px 
                    img 
                        border-radius 50%
                .content
                    flex:1
                    position relative
                    .name
                        font-size 10px
                        line-height:12px 
                        color:rgb(7,17,27) 
                        margin-bottom 4px 
                    .star-wrapper
                        margin-bottom 6px 
                        font-size 0
                        .star
                            margin-right:6px 
                            display inline-block
                            vertical-align top 
                        .delivery
                            display inline-block
                            vertical-align top
                            font-size 10px
                            line-height:12px 
                            color:rgb(147,153,159)  
                    .text
                        font-size 12px 
                        line-height:18px 
                        color:rgb(7,17,27)
                        margin-bottom 8px         
                    .recomend
                        line-height 16px 
                        .item ,.icon-thumb_up
                            display inline-block
                            margin:0 8px 4px 0
                        .icon-thumb_up
                            font-size 12px 
                            color:rgb(0,160,220)
                            margin-right:8px 
                        .item
                            font-size 9px 
                            line-height 16px 
                            border:1px solid rgba(7,17,27,.1) 
                            border-radius 2px 
                            background:#fff
                            color:rgb(147,153,159)  
                    .time
                        position absolute
                        right:18px 
                        top:18px 
                        font-size 10px
                        line-height:12px 
                        color:rgb(147,153,159) 

</style>
