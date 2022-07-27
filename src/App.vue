<template>
  <div id="app">
   
<HeaderVue></HeaderVue>
<MainVue></MainVue>
   
  </div>
</template>

<script>

import HeaderVue from './components/HeaderVue.vue'
import MainVue from './components/MainVue.vue'
export default {
  name: 'App',
 components:{
  HeaderVue,
  MainVue
 }
}
</script>

<style>
*{
   box-sizing: border-box;
   padding:0;
   margin:0;
}
body{
  background: #171717;
  color: white;
 font-size: 14px;
 font-family: 'Poppins', sans-serif;
  
}
.container{
  max-width: 1040px;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
span{
    color: #ffaf02;
    text-transform: uppercase;
}
@media only screen and (max-width: 600px) {
  body {
     font-size: 10px;
     padding: 20px 0px;
  
  }
}
</style>
