<template>
    <div class="header">
        <div class="content-warpper">
            <div class="avatar">
                <img width="64" height="64" :src="seller.avatar">
            </div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description+'/'+seller.deliveryTime+'分钟送达'}}
                </div>
                <div v-if="seller.supports" class="support">
                    <span class="icon" :class="classMap[seller.supports[0].type]"></span>
                    <span>{{seller.supports[0].description}}</span>
                </div>
            </div> 
            <div v-if="seller.supports" class="supports-count" @click="Showdateil">
                <span class="count">{{seller.supports.length+'个'}}</span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        <div class="bulletin-warpper" @click="Showdateil">
            <span class="bulletin-title"></span>
            <span class="bulletin-text">{{seller.bulletin}}</span>
            <i class="icon-keyboard_arrow_right"></i>
        </div>
        <div class="bg">
            <img :src="seller.avatar" width="100%" height="100%">
        </div>
        <div v-show="detailShow" class="detail">
            <div class="detail-warpper clearfix">
                <div class="detail-main">
                    <p class="name">{{seller.name}}</p>
                    <!-- 星星组件 -->
                    <div class="star-warpper">
                        <star :size='48' :score="seller.score"></star>
                    </div>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">优惠信息</div>
                        <div class="line"></div>
                    </div>
                    <ul v-if="seller.supports" class="supports">
                        <li class="support-item" v-for="(item,index) in seller.supports" :key="index">
                        <span class="icon" :class="classMap[seller.supports[index].type]"></span>
                        <span class="text">{{seller.supports[index].description}}</span>
                        </li>
                    </ul>
                    <div class="title">
                        <div class="line"></div>
                        <div class="text">商家公告</div>
                        <div class="line"></div>
                    </div>
                    <div class="bulletin">
                        <p class="content">{{seller.bulletin}}</p>
                    </div>
                </div>
            </div>
            <div class="detail-close" @click="hideDetail">
                <i class="icon-close"></i>
            </div>
        </div>
    </div>
</template>

<script>
import star from 'components/star/star'
export default {
    name:'HeaderTop',
    props:{
        seller:{
            type:Object
        }
    },
    data(){
        return {
            detailShow:false
        }
    },
    methods:{
        Showdateil (){
            this.detailShow = true
        },
        hideDetail (){
            this.detailShow = false
        }
    },
    created (){
        this.classMap=['decrease','discount','guarantee','invoice','special']
    },
    components:{
        star
    }
}
</script>

<style lang="stylus" >
@import '~common/stylus/mixin.styl'    
    .header
        position relative
        color:#fff
        background rgba(7,17, 27, .5)
        font-size 0
        z-index 50
        .content-warpper
            position relative
            padding 24px 12px 18px 24px
            .avatar
                display inline-block
            .content
                display inline-block    
                font-size 14px
                margin-left:14px
                .title
                    margin 2px  0px 8px 0px
                    vertical-align top
                    .brand
                        bg-image('brand')
                        height 18px
                        width 30px
                        display inline-block
                        background-size:30px 18px
                        background-repeat no-repeat
                    .name
                        margin-left:6px
                        font-size 16px
                        line-height:18px    
                .description
                    font-size 12px
                    line-height:12px
                .support
                    margin-top 10px
                    .icon
                        vertical-align top
                        display inline-block
                        margin-right:4px
                        width:12px
                        height 12px
                        background-size 12px
                        background-repeat no-replace
                        &.decrease
                            bg-image('decrease_1')
                        &.discount
                            bg-image('discount_1')
                        &.guarantee
                            bg-image('guarantee_1')   
                        &.invoice
                            bg-image('invoice_1')
                        &.special
                            bg-image('special_1')    
                    .text
                        font-size 12px
                        line-height:12px                 
            .supports-count
                position:absolute
                right 12px
                bottom 14px
                padding 0 8px
                height 24px
                line-height:24px
                border-radius 14px
                background:rgba(0,0,0,.2)
                text-align:center
                .count
                    vertical-align top
                    font-size 10px
                .icon-keyboard_arrow_right    
                    font-size 10px
                    line-height 24px
                    margin-left:2px
        .bulletin-warpper
            height 28px
            line-height:28px 
            padding 0 22px 0 12px
            white-space nowrap
            overflow hidden
            text-overflow ellipsis
            font-size:0
            background:rgba(0,0,0, .2)
            position relative
            .bulletin-title
                margin-top:8px
                display inline-block
                vertical-align top
                width:22px
                height 12px
                bg-image('bulletin')
                background-repeat no-replace
                background-size 22px 12px 
            .bulletin-text
                vertical-align top
                margin:0 4px
                font-size 10px
            .icon-keyboard_arrow_right    
                font-size:12px
                position absolute
                right 12px
                top:8px
        .bg
            width:100%
            height 100%
            z-index -1
            position:absolute
            top:0
            left:0
        .detail
            position fixed
            z-index 100
            width:100%
            height:100%
            overflow auto 
            background rgba(7,17,27,.8)
            top:0
            left 0
            .detail-warpper
                min-height 100%
                width 100%
                .detail-main
                    margin-top 64px
                    padding-bottom 60px
                    .name
                        font-size:16px
                        line-height:16px 
                        color:#fff
                        font-weight 700
                        text-align center
                    .star-warpper
                        margin-top:18px 
                        padding 2px 0 
                        text-align center 
                    .title
                        width 80%
                        margin 30px auto 24px auto 
                        display flex
                        .line
                            flex 1
                            position relative
                            top:-6px
                            border-bottom:1px solid rgba(255,255,255,.2)
                        .text
                            padding 0 12px 
                            font-size:14px
                            font-weight 700
                    .supports
                        width:80%
                        margin:0 auto 
                        .support-item
                            padding 0 12px
                            margin-bottom 12px
                            font-size:0
                            &:last-child
                                margin-bottom 0
                            .icon
                                display inline-block
                                height 16px
                                width:16px
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
                                line-height:12px
                                font-size:12px 
                    .bulletin
                        width:80%
                        margin:0 auto 
                        .content
                            padding 0 12px 
                            line-height:24px 
                            font-size 12px


            .detail-close
                position relative
                width:32px
                height:32px
                margin:-60px auto 0 auto 
                clear both
                font-size:32px


</style>
