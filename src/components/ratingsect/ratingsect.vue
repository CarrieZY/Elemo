<template>
    <div class="ratingselect">
        <div class="rating-type border-1px">
            <span @click="select(2,$event)" class="block posttive" :class="{'active':selectType==2}">{{desc.all}}
                <span class="count">{{ratings.length}}</span>
            </span>
            <span @click="select(0,$event)" class="block posttive" :class="{'active':selectType==0}">{{desc.posttive}}
                <span class="count">{{posttives.length}}</span>
            </span>
            <span @click="select(1,$event)" class="block negative" :class="{'active':selectType==1}">{{desc.negative}}
                <span class="count">{{negatives.length}}</span>
            </span>
        </div>
        <div @click="toggleContent" class="switch" :class="{'on':onlyContent}">
            <span class="icon-check_circle"></span>
            <span class="text">只看有内容的评价</span>
        </div>
    </div>
</template>

<script>
const posttive=0
const negative=1
const all =2
export default {
    name:'ratingsect',
    props:{
        ratings:{
            type:Array,
            default(){
                return []
            }
        },
        selectType:{
            type:Number,
            default:all
        },
        onlyContent:{
            type:Boolean,
            default:false
        },
        desc:{
            type:Object,
            default(){
                return {
                    all:'全部',
                    posttive:'满意',
                    negative:'不满意'
                }
            }
        }
    },
    computed:{
        //计算满意的数量
        posttives(){
            return this.ratings.filter((rating) =>{
                return rating.rateType===posttive
            })
        },
        //计算不满意的数量
        negatives(){
            return this.ratings.filter((rating) =>{
                return rating.rateType===negative
            })
        }
    },
    methods:{
        select(type,event){
            if(!event._constructed){
                return
            }
            this.selectType=type
            //子组件告诉父组件的变化
            this.$nextTick('ratingtype.select',type)
        },
        toggleContent(event){
           if(!event._constructed){
                return
            } 
            this.onlyContent= !this.onlyContent
            this.$nextTick('content.toggle',this.onlyContent)
        }
    }
}
</script>

<style lang="stylus" scoped>
@import '~common/stylus/mixin.styl'
.ratingselect
    .rating-type
        padding:18px 0
        margin:0 18px
        border-1px(rgba(7,17,27,.1))
        font-size:0
        .block
            display inline-block
            padding 8px 12px
            margin-right:8px 
            border-radius 1px
            font-size 12px
            color:rgb(77,85,93) 
            background:rgb(0,160,220)
            &.active
                color:#fff
            .count
                font-size:8px 
                margin-left:2px 
                line-height:16px
            &.posttive
                background:rgba(0,160,220,.2)
                &.active
                    background rgb(0,160,220)
            &.negative
                background rgba(77,85,93,.5)
                &.active
                 background rgb(77,85,93)    
    .switch
        padding 12px 18px
        border-bottom:1px solid rgba(7,17,27,.1)
        line-height 24px
        color:rgb(147,153,159)
        font-size 0
        &.on
            .icon-check_circle
                color:#00c850
        .icon-check_circle
            display inline-block
            vertical-align top
            font-size:24px
            margin-right 4px
        .text
            display inline-block
            vertical-align top
            font-size 12px    

               

</style>
