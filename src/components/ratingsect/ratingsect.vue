<template>
    <div class="ratingselect">
        <div class="rating-type border-1px">
            <span class="block all">{{desc.all}}
                <span class="count">47</span>
            </span>
            <span class="block posttive">{{desc.posttive}}
                <span class="count">40</span>
            </span>
            <span class="block negative">{{desc.negative}}
                <span class="count">7</span>
            </span>
        </div>
        <div class="switch">
            <span class="icon-check_circle"></span>
            <span class="text">只看有内容的评价</span>
        </div>
    </div>
</template>

<script>
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
            color:rgb(147,159,220)
            font-size 0
            &.icon-check_circle
                display inline-block
                vertical-align top
                font-size:24px
                margin-right 4px
            .text
                font-size 12px    

               

</style>
