<template>
<div class="main">
  <div v-for="(num,index) in (floorCount+1)" :key="index">
    <div :style="{top:`${(index*100) +5}px`}" class="grid">
    </div>
  </div>
  <div class="floors" ref="floors" :style="{height:`${this.floorCount*100}px`}">
    <div class="elevator" ref="elevator" >
    </div>
  </div>
  <div v-for="(num,index) in floorCount" :key="index">
    <button :style="{top:`${(index*100) +45}px`}" class="elevator-button" @click="btnhandler(num)">
    </button>
  </div>
</div>
</template>

<script>
// import ElevatorMain from './components/ElevatorMain.vue'

export default {
  name: 'App',
  components: {
    // ElevatorMain
  },
  data() {
    return {
      floorCount:7,
      curentFloor: 0
    }
  },
  methods: {
    btnhandler(floorNumber){
      console.log(Math.abs(this.curentFloor - (this.floorCount - floorNumber)))
      this.$refs.elevator.style.top=`${(floorNumber-1)*100}px`
      this.$refs.elevator.style.transitionDuration=`${Math.abs(this.curentFloor - (this.floorCount - floorNumber))}s`
      this.curentFloor = this.floorCount - floorNumber
    }
  },
}
</script>

<style>
.main{
  display: flex;
}
body{
  background: rgb(232, 232, 232);
}
*{
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
.floors{
  margin-top: 5px;
  margin-left: 30px;
  width: 102px;
  border: 1px solid black;
}
.elevator{
  transition: all linear;
  transition-duration: 1s;
  position: relative;
  background: coral;
  width: 100%;
  height: 99px;
  top: 600px;
}
.grid{
  position: absolute;
  width: 100%;
  height: 1px;
  background: rgb(212, 212, 212);
}
.elevator-button{
  position:absolute;
  width: 20px;
  height: 20px;
  border-radius: 100%;
}
</style>
