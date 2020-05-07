<template>
  <div class="home">
    <div class="left" @click="openManager">
      <div class="left-upper">
        <img class="pzl" src="@/assets/upperPuzzle.svg" alt="" id="upperPuzzle">
      </div>

      <div class="left-lower">
        <img class="pzl" src="@/assets/puzzlefill.svg" alt="" id="leftCompassFill">
        <img class="pzl" src="@/assets/lowerPuzzle.svg" alt="" id="lowerPuzzle">
        <p class="to">Manage Categories</p>
      </div>
    </div>

    <div class="right" @click="openPathCreator">
      <div class="right-upper">
        <img class="cps" src="@/assets/compassLeft.svg" alt="" id="leftCompass">
      </div>

      <div class="right-lower">
        <img class="cps" src="@/assets/compassLeft.svg" alt="" id="rightCompass">
        <p class="to" id="toCreator" >Create Path</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods:{
    openPathCreator(){
      document.querySelector('.home').classList.add('openPathCreator')
      this.removeHome();
      this.moveLogo();
    },
    openManager(){
      document.querySelector('.home').classList.add('openManager');
      this.removeHome();
      this.moveLogo();
    },
    removeHome(){
      setTimeout(()=>{
        const home = document.querySelector('.home');
        home.style.transform = 'scale(0)'
        home.style.top = '-100%'
        home.classList.remove('openPathCreator');
        home.classList.remove('openManager');
        console.log(home);
      },1000)
    },
    moveLogo(){
      document.querySelector('#logo').style.left="3%"
      document.querySelector('#logo').style.top="10px"
      document.querySelector('#logo').style.width="5%"
    }
  }
}
</script>

<style>
.home{
  position: absolute;
  width: 100%;
  display: flex;
  top:0%;
  bottom: 0%;
  height: 100vh;
  transition: 0.2s top;
  z-index: 3;
  overflow: hidden;
}
.home img{
  width: 100px;
  left:50%;
  transform: translateX(-50%);
  position: absolute;
  z-index: 3;
}
.left{
  /* border-right: 2px solid rgb(175, 122, 177); */
  width: 50%;
}
.right{
  border-left: 2px solid rgb(171, 148, 173);
  width: 50%;
}
.left-upper,.left-lower,.right-upper,.right-lower{
  position: relative;
  height: 50vh;
}
.right,.left{
  transition: 0.4s;
  text-align: center;
}
.right{
}
.right img{
  transition: 0.4s ease-in-out;

}
.right-upper,.right-lower,.left-lower,.left-upper{
  transition: 1s;
  transition-delay: 0.6s;
  top:0%;
  bottom: 0%;

}
.right-upper{
  background: linear-gradient(0deg,#1F2C40,#0F0F2A);
}
.right-lower{
  background: linear-gradient(180deg,#1F2C40,#0F0F2A);
}
.right-upper img{
  bottom: -100px;
  left: 35%;
  transform: rotate(45deg) translateX(-50%);
}
.right-lower img{
  left: 34.75%;
  top:-102.5px;
  transform: rotate(225deg) translateX(-50%);
}
.right:hover #leftCompass{
  transform: rotate(360deg) translateX(-50%);
}
.right:hover #rightCompass{
  transform: rotate(540deg) translateX(-50%);
  top:-100.1px;
  left: 34.7%;
}
.right:hover .to{
  transform: translateY(300%) translateX(50%) scale(1);
}
.to{
  color: white;
  font-size: 2em;
  background: rgba(255, 255, 255, 0.144);
  width: 50%;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  padding: 10px;
  transform: translateY(0%) translateX(50%) scale(0);
  border-radius: 30px;
  transition: 0.4s;
}
.to:hover{
  cursor: pointer;
}
#toCreator{
  margin-left: -40px;
}



.left-upper img{
  bottom: -2px;
}
.left-lower img{
  top: 0px;
  width: 133px;
  margin-left: 16px;
  /* background: rgb(0, 132, 255); */
}
#leftCompassFill{
  width: 95px;
  margin-left: -3px;
  top:80px;
  z-index: 5;
  opacity: 0;
  transition: 0.4s top ease ;
}
.left-upper{
  background: linear-gradient(180deg,rgb(32, 21, 48),#3F2E5A);
}
.left-lower{
  background: linear-gradient(0deg,rgb(32, 21, 48),#3F2E5A);
}
.left:hover #leftCompassFill{
  top:12.5px;
  opacity: 1;
}
.left:hover .to{
  transform: translateY(300%) translateX(50%) scale(1);
}



.openPathCreator{
  grid-template-columns: none;
}

.openPathCreator .left{
  width: 0%;
  height: 0;
}
.openPathCreator .right{
  width: 100%;
}
.openPathCreator .right:hover #rightCompass{
  left: 45%;
  margin-top: -2px;
}
.openPathCreator #leftCompass{
  transform:rotate(90deg) translateX(-50%) !important;
  left: 45%;
}
.openPathCreator #rightCompass{
  transform:rotate(-90deg) translateX(-50%) !important;
  left:45%;
}
.openPathCreator .right-upper{
  width: 100%;
  top:-100%
}
.openPathCreator .right-lower{
  width: 100%;
  top:100%
}
.openPathCreator .pzl{
  display: none;
}


.openManager{
  grid-template-columns: none;
}
.openManager .right{
  width: 0%;
  height: 0;
}
.openManager .left{
  width: 100%;
}
.openManager .left-upper{
  width: 100%;
  top:-100%;
}
.openManager .left-lower{
  width: 100%;
  top:100%;
}
.openManager .cps{
  display: none;
}
</style>