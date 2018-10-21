<template>
    <div class="goods">
        <div class="menu-warpper" ref="menuWrapper">
            <ul>
                <li v-for="(item,index) in goods" class="menu-item" :key="index" 
                :class="{'current': currentIndex === index }" @click="selectMenu(index)">
                    <span class="text border-1px">
                        <span v-show="item.type>0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
                    </span>
                </li>
            </ul>
        </div>
        <div class="foods-warpper" ref="foodWrapper">
            <ul>
                <li v-for="(item,index) in goods" :key="index" class="food-list food-list-hook">
                    <p class="title">{{item.name}}</p>
                    <ul>
                        <li v-for="(food ,index) in item.foods" :key="index" class="food-item border-1px">
                            <div class="icon">
                                <img :src="food.icon" width="57" height="57">
                            </div>
                            <div class="content">
                                <p class="name">{{food.name}}</p>
                                <p class="desc">{{food.description}}</p>
                                <div class="extra">
                                    <span class="count">{{'月售'+food.sellCount+'份'}}</span>
                                    <span>{{'好评率'+food.rating+'%'}}</span>
                                </div>
                                <div class="price">
                                    <span class="now">{{'￥'+food.price}}</span>
                                    <span v-show="food.oldPrice" class="old">{{'￥'+food.oldPrice}}</span>
                                </div>
                            </div>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <shopcart></shopcart>
    </div>
</template>

<script>
import BScroll from 'better-scroll'
import shopcart from 'components/shopcart/shopcart'
import axios from 'axios'
export default {
    name:'goods',
    components:{
        shopcart
    },
    props:{
        seller:{
            type:Object
        }
    },
    data (){
        return {
        goods: [],
        listHeight: [],
        scrollY: 0
      }
    },
    created(){
       axios.get('static/data.json').then( (res) =>{
            console.log(res)
            this.goods=res.data.goods
            this.$nextTick(() =>{
                this.initScroll()
                this.calculateHeight()
            })
        })
        this.classMap=['decrease','discount','guarantee','invoice','special']        
    },
    methods:{
        selectMenu(index){
            console.log(index)
            let foodlist = this.$refs.foodWrapper.getElementsByClassName('food-list-hook');
            this.scroll.scrollToElement(foodlist[index], 250) 
             //better-scroll的scrollToElement方法滚动到指定位置
        },
        initScroll(){
             this.scroll=new BScroll(this.$refs.foodWrapper,{
                 probeType:3
             })
             this.menuScroll= new BScroll(this.$refs.menuWrapper,{
                 click:true
             })
             this.scroll.on('scroll',(pos) =>{
                 this.scrollY = Math.abs(Math.round(pos.y))
             })
        },
        //定义一个计算高度的方法  在上面调用
        calculateHeight(){
            //获取每个li
            let foodList = this.$refs.foodWrapper.getElementsByClassName('food-list-hook')
            console.log(foodList)
            let Height=0
            this.listHeight.push(Height)
            //循环
            for (let i=0;i<foodList.length;i++ ){
                 let item = foodList[i] //每一个item都是刚才获取的food的每一个dom
                 Height += item.clientHeight //主要是为了获取每一个foods内部块的高度
                 this.listHeight.push(Height)
            }
        }
    },
    computed: {
        currentIndex(){ //计算到达哪个区域的区间的时候的对应的索引值
            for (let i = 0; i < this.listHeight.length; i++) {
            let height1 = this.listHeight[i]; //当前menu子块的高度
            let height2 = this.listHeight[i + 1]; //下一个menu子块的高度
            //滚动到底部的时候,height2为undefined,需要考虑这种情况
            //需要确定是在两个menu子块的高度区间
            if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
                return i //返回这个menu子块的索引
            }
            }
            return 0;
        }
    }
}
</script>

<style lang="stylus" scoped>
@import '~common/stylus/mixin.styl'
.goods
    display flex
    width:100%
    position:absolute
    top:174px 
    bottom 46px
    overflow hidden
    .menu-warpper
        flex 0 0 80px
        background:#f3f5f7
        width:80px
        .menu-item
            display table
            height:54px
            width:56px
            line-height:14px 
            padding 0 12px 
            &.current
                position relative
                margin-top:-1px 
                z-index 10
                background:#fff
                font-weight:700
                .text
                    border-none()
            .icon
                display: inline-block
                vertical-align: top
                width: 12px
                height: 12px
                margin-right: 2px
                background-size: 12px 12px
                background-repeat: no-repeat
                &.decrease
                    bg-image('decrease_3')
                &.discount
                    bg-image('discount_3')
                &.guarantee
                    bg-image('guarantee_3')
                &.invoice
                    bg-image('invoice_3')
                &.special
                    bg-image('special_3')
                .text
                    display table-cell
                    width:56px 
                    vertical-align middle
                    border-1px(rgba(7, 17,27,0.1))
                    font-size:12px
    .foods-warpper
        flex 1  
        .title
            padding-left 14px
            height:26px
            line-height:26px
            border-left:2px solid #d9dbd9
            font-size:12px
            color:rgb(147, 157,159)
            background:#f3f5f7
        .food-item
            display flex
            margin:18px
            border-1px(rgba(7, 17,27,0.1))  
            padding-bottom 18px
            &:last-child
                border-none()  
                margin-bottom 0
            .icon
               flex 0 0 57px
               margin-right:10px
             .content
                flex: 1
                .name
                    margin: 2px 0 8px 0
                    height: 14px
                    line-height: 14px
                    font-size: 14px
                    color: rgb(7, 17, 27)
                .desc,.extra 
                    line-height:10px 
                    font-size:10px 
                    color:rgb(147,153,159)  
                .desc
                    margin-bottom 8px    
                .extra
                 &:.count
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

    // .scoped
</style>
