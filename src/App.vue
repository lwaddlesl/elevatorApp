<template>
  <main class="main">
    <div v-for="(num, index) in floorCount + 1" :key="index">
      <div :style="{ top: `${index * 100 + 5}px` }" class="grid"></div>
    </div>
    <div
      class="floors"
      ref="floors"
      :style="{ height: `${this.floorCount * 100}px` }"
    >
      <div 
        class="elevator" 
        :class="{blink: isBlink}" 
        ref="elevator"
      ></div>
    </div>
    <div v-for="(num, index) in floorCount" :key="index">
      <div
        :style="{ top: `${index * 100 + 45}px` }"
        class="elevator-button"
        :class="{active: isActive.includes(num)}"
        @click="btnHandler(num, true)"
      ></div>
    </div>
  </main>
</template>

<script>
// import ElevatorMain from './components/ElevatorMain.vue'

export default {
  name: "App",
  components: {
    // ElevatorMain
  },
  watch: {
    isBusy(){
      if (this.isActive.length && !this.isBusy){
        this.btnHandler(this.isActive[0], false)
      }
    }
  },
  data() {
    return {
      floorCount: 6,
      curentFloor: 0,
      isActive: [],
      isBlink:null,
      isBusy: false
    };
  },
  mounted() {
    this.$refs.elevator.style.top = `${(this.floorCount-1) * 100}px`
  },
  methods: {
    async btnHandler(floorNumber,click) {
      if(this.curentFloor == (this.floorCount - floorNumber)){
        return
      }
      if( this.isActive.includes(floorNumber) && click){
        return
      }
      if (this.isBusy){
        this.isActive.push(floorNumber)
        return
      }
      this.isBusy = true
      if (click) this.isActive.push(floorNumber)
      let time = Math.abs(this.curentFloor - (this.floorCount - floorNumber))
      this.$refs.elevator.style.top = `${(floorNumber - 1) * 100}px`;
      this.$refs.elevator.style.transitionDuration = `${time}s`;
      this.curentFloor = this.floorCount - floorNumber;
      await new Promise(resolve => setTimeout(resolve, time*1000))
      let index = this.isActive.indexOf(floorNumber)
      console.log(this.isActive)
      this.isActive.splice(index,1)
      this.isBlink = true
      await new Promise(resolve => setTimeout(resolve, 3000))
      this.isBlink = false
      this.isBusy = false
    },
  },
};
</script>

<style>
.main {
  display: flex;
}
body {
  background: rgb(232, 232, 232);
}
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
.floors {
  margin-top: 5px;
  margin-left: 30px;
  width: 102px;
  border: 1px solid black;
}
.elevator {
  transition: all linear;
  transition-duration: 1s;
  position: relative;
  background: coral;
  width: 100%;
  height: 99px;
}
.grid {
  position: absolute;
  width: 100%;
  height: 1px;
  background: rgb(212, 212, 212);
}
.elevator-button {
  border: 1px solid black;
  position: absolute;
  width: 20px;
  margin-left: 5px;
  height: 20px;
  border-radius: 100%;
  cursor: pointer;

}
.active{
  background: red;
}
.blink {
  animation: .5s linear 0s infinite alternate blink-animation;
  -webkit-animation: .5s linear 0s infinite alternate blink-animation;
}
@keyframes blink-animation {
  from {
    opacity: 100%;
  }
  to {
    opacity: 20%;
  }
}
@-webkit-keyframes blink-animation {
  from {
    opacity: 100%;
  }
  to {
    opacity: 40%;
  }
}
</style>
