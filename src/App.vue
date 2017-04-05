<template>
  <div id="app">
    <div id="wheel-wrapper"></div>
    <div id="fortune-wheel"></div>

    <button v-on:click="spin()">Keeruta</button>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      angle: 0
    }
  },
    methods: {
      spin: function () {
          let max = 200;
          let min = 12;
          let rand = Math.floor(Math.random() * (max - min + 1)) + min;

          this.angle = this.angle + (30 * rand);

          let wheel = document.getElementById("fortune-wheel");
          wheel.style.transform = "rotate(-" + this.angle + "deg)";
      }
    }
}
</script>

<style lang="scss">
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    vertical-align: top;
  }

  body, html {
    height: 100%;
  }

  #wheel-wrapper {
    width: 600px;
    height: 811px;
    background: url("./assets/wheel_wrapper.png") no-repeat center center;
    background-size: contain;
    position: absolute;
    bottom: 50px;
    left: 50%;
    margin-left: -300px;
    z-index: 100;
  }

  #fortune-wheel {
    width: 550px;
    height: 550px;
    background: #cccccc;
    position: absolute;
    border-radius: 50%;
    background: url("./assets/wheel.png") no-repeat center center;
    background-size: contain;
    z-index: 10;
    bottom: 280px;
    left: 50%;
    margin-left: -275px;
    cursor: pointer;
    transition-duration: 1s;
    transition-property: transform;
    transition-timing-function: ease-in-out;
  }
</style>
