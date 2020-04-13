<template>
  <div id="app">
    <img @click="showHome" src="@/assets/logo.svg" alt="" id="logo">
    <pathCreator/>
    <home/>
  </div>
</template>

<script>
import home from './components/home.vue'
import pathCreator from '@/components/pathCreator.vue'

export default {
  name: 'App',
  components: {
    home,
    pathCreator
  },
  methods:{
    showHome(){
        const home = document.querySelector('.home');
        home.style.display = 'flex'
        home.style.transform = 'scale(1)'
        home.style.top = '0%';
        this.centerLogo();
    },
    centerLogo(){
      document.querySelector('#logo').style.left="50%"
      document.querySelector('#logo').style.top="0px"
      document.querySelector('#logo').style.width="10%"

    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding:0;
  box-sizing: border-box;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}
body{
  background: #000019;
}
#logo{
  position: fixed;
  top:0px;
  left:50%;
  transform: translateX(-50%);
  z-index: 10;
  width: 10%;
  transition: 0.2s ease;
}
</style>
