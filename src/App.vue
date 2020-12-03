<template>
  <div id="app">
    <Navbar/>
    <router-view/>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar'
export default {
  components:{
    Navbar
  },
  data(){
    return {

    }
  },
  mounted(){
    document.body.style.background = '#fffefc'
    // document.body.style.background = '#000000'
    document.body.style.fontSize = '16px'
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>
