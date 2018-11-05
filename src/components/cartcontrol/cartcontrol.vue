<template>
    <div class="cartcontrol">
        <transition name="move">
            <div class="minus " v-show="food.count>0" 
            @click.stop.prevent="minusCart">
            <span class="inner icon-remove_circle_outline"></span>
            </div>
        </transition>
        <div class="count" v-show="food.count>0">{{food.count}}</div>
        <div class="add icon-add_circle" @click.stop.prevent="addCart"></div>
    </div>
</template>

<script>
import Vue from 'vue'
export default {
    name:'cartcontrol',
    props:{
        food:{
            type:Object
        }
    },
    created(){
        console.log(this.food)
    },
    methods:{
        addCart(){
            console.log('click')
            if(!this.food.count){
               Vue.set(this.food,'count',1)
            }else{
                this.food.count++
            }
        },
        minusCart(){
            console.log('click')
            if(this.food.count){
                this.food.count--
            }else{
               Vue.set(this.food,'count',0) 
            }
        }
    }
}
</script>

<style lang="stylus" scoped>
.cartcontrol
  font-size 0
  &.cartcontrol > div 
    display inline-block
  .minus
    padding 6px
    transition  all 0.4s linear 
    &.move-enter-active
        opacity :1
        transform translate3d(0,0,0)
    .inner
        display inline-block
        line-height:24px
        font-size:24px
        color:rgb(0,160,220)
        transition all 0.4s linear 
        transform rotate(0)
    &.move-enter,&.move-leave
        opacity 0
        transform translate3d(24px,0,0) 
        .inner
           transform rotate(180)    
  .count
    width:12px 
    padding-top:6px
    vertical-align top
    line-height:24px 
    font-size 10px
    color:rgb(147,153,159) 
  .add
      padding 6px
      line-height:24px 
      font-size:24px
      color:rgb(147,153,159) 
</style>
