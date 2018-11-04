<template>
  <div id="app">
    <header-top :seller="seller"></header-top>
    <tab></tab>
      <router-view :seller="seller"/>
  </div>
</template>

<script>
import HeaderTop from 'components/header/header'
import Tab from 'components/tab/tab'
import axios from 'axios'

export default {
  name: 'App',
  components:{
    HeaderTop,
    Tab
  },
  data(){
    return {
      seller:{}
    }
  },
  created(){
    axios.get('static/data.json').then( (res) =>{
      console.log(res)
      this.seller=res.data.seller
    })
  }
}
</script>

<style lang="stylus">
</style>
