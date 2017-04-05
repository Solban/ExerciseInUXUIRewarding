<template>
  <div id="app">
    <div id="prediction-wrapper">
        <div class="prediction-title">Ennusta, milline värv õnnestub sul välja keerutada:</div>
        <div class="prediction" v-for="color in colors" v-on:click="prediction = color.hex" v-bind:style="{backgroundColor: color.hex}" v-bind:class="{ active: prediction === color.hex }"></div>
    </div>
      <div id="arrow-wrapper">
          <div id="arrow"></div>
          <div id="fortune-wheel"></div>
    </div>
    <button v-on:click="spin()">Keeruta</button>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      colors: [
          { "hex": "#8fc31f", "name": "light green" },
          { "hex": "#ffff00", "name": "yellow" },
          { "hex": "#f39800", "name": "orange" },
          { "hex": "#e60012", "name": "red" },
          { "hex": "#e5004f", "name": "dark pink" },
          { "hex": "#e4007f", "name": "pink" },
          { "hex": "#920783", "name": "violet" },
          { "hex": "#1d2088", "name": "dark blue" },
          { "hex": "#0068b7", "name": "blue" },
          { "hex": "#00a0e9", "name": "light blue" },
          { "hex": "#009e86", "name": "green" },
          { "hex": "#009944", "name": "dark green"}
      ],
      prediction: "#8fc31f",
      angle: 15
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

          let color_index = (((this.angle - 15) / 30) % 12) + 1;
          if (color_index === 12) color_index = 0;
          let color = this.colors[color_index];
          console.log(color["name"]);
          if (color["hex"] === this.prediction) {
              console.log("Wohoo!!! Võitsid miljon eurot!");
          } else {
              console.log("Böööö! Kaotasid!");
          }
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
      font-family: "Helvetica Neue", Helvetica, Arial, sans-serif;
      color: #5d5d5d;
  }

  body, html {
      height: 100%;
  }

  body {
      overflow-y: scroll;
  }

  #app {
      width: 620px;
      margin: 0 auto;
  }

  #arrow-wrapper {
      width: 570px;
      height: 570px;
      margin: 0 auto;
      background: #5d5d5d;
      border-radius: 50%;
      padding: 10px;
      position: relative;
  }

  #arrow {
      width: 0;
      height: 0;
      border-left: 20px solid transparent;
      border-right: 20px solid transparent;
      border-top: 20px solid #5d5d5d;
      position: absolute;
      left: 50%;
      margin-left: -20px;
      z-index: 10;
  }

  #fortune-wheel {
    width: 550px;
    height: 550px;
    margin: 0 auto;
    background: #cccccc;
    border-radius: 50%;
    background: url("./assets/wheel2.png") no-repeat center center;
    background-size: contain;
    transition-duration: 3s;
    transition-property: transform;
    transition-timing-function: ease-out;
    transform: rotate(15deg);
  }

  #prediction-wrapper {
      padding: 25px 10px;
      font-weight: bold;
  }

  .prediction-title {
      margin: 15px 0;
      text-align: center;
      font-size: 20px;
  }

  .prediction {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      margin: 5px;
      display: inline-block;
      cursor: pointer;
      border: 5px solid #fff;
  }

  .prediction:hover, .prediction.active {
      border: none;
  }

  button {
      border: none;
      width: 100%;
      height: 40px;
      margin: 40px 0 15px 0;
      font-size: 16px;
      color: #fff;
      cursor: pointer;
      font-weight: bold;
      background: #00a0e9;
  }

  button:hover {
      background: #008dd3;
  }

  button:focus {
      outline: none;
  }
</style>
