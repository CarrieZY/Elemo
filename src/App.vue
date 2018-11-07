<template>
  <div id="app">
    <header-top :seller="seller"></header-top>
    <tab></tab>
    <!-- keep=alive减少http请求  防止二次渲烂 -->
   <keep-alive>    
      <router-view :seller="seller"/>
    </keep-alive>
  </div>
</template>

<script>
import HeaderTop from 'components/header/header'
import Tab from 'components/tab/tab'
import axios from 'axios'
import {urlParse} from 'common/js/ulit'
export default {
  name: 'App',
  components:{
    HeaderTop,
    Tab
  },
  data(){
    return {
      seller:{
        //立即执行函数取到商家的id传到URL上
        id:(() =>{
          let queryParam = urlParse()
          return queryParam.id
        })()
      }
    }
  },
  created(){
    axios.get('static/data.json?id='+this.seller.id).then( (res) =>{
      console.log(res)
      this.seller=res.data.seller
    })
  }
}
</script>

<style lang="stylus">
</style>
