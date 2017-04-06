<template>
  <div id="app">
    <audio id="loss-audio" src="../Helid/loss3.mp3" autostart="false" ></audio>
    <audio id="win-audio" src="../Helid/win_big.mp3" autostart="false" ></audio>
    <div class="overlay" v-if="state === 'win' || state === 'lose'">
      <div class="win" v-if="state === 'win'">
        <span class="text">
          <span>S</span>
          <span>A</span>
          <span class="space"></span>
          <span>V</span>
          <span>Õ</span>
          <span>I</span>
          <span>T</span>
          <span>S</span>
          <span>I</span>
          <span>D</span>
          <span>!</span>
        </span>
        <button @click="reset">Jätka mängimist</button>
      </div>
      <div class="lose" v-if="state === 'lose'">
        <span class="text">
          <span>S</span>
          <span>A</span>
          <span class="space"></span>
          <span>K</span>
          <span>A</span>
          <span>O</span>
          <span>T</span>
          <span>A</span>
          <span>S</span>
          <span>I</span>
          <span>D</span>
          <span>!</span>
        </span>
        <button @click="reset">Jätka mängimist</button>
      </div>
    </div>
    <div id="prediction-wrapper">
        <div class="prediction-title">Ennusta, milline värv õnnestub sul välja keerutada:</div>
        <div class="prediction" v-for="color in colors" v-on:click="prediction = color.hex" v-bind:style="{backgroundColor: color.hex}" v-bind:class="{ active: prediction === color.hex }"></div>
    </div>
    <div id="arrow-wrapper">
      <div id="arrow"></div>
      <div class="wheel-wrapper">
        <div id="fortune-wheel"></div>
        <button class="spin" v-on:click="spin" v-if="state === 'fresh'">Keeruta</button>
      </div>
    </div>
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
      angle: 15,
      state: 'fresh'
    }
  },
    methods: {
      spin: function () {
          this.state = 'spinning';
          let max = 200;
          let min = 12;
          let rand = Math.floor(Math.random() * (max - min + 1)) + min;

          this.angle = this.angle + (30 * rand);

          let wheel = document.getElementById("fortune-wheel");
          wheel.style.transform = "rotate(-" + this.angle + "deg)";

          let color_index = (((this.angle - 15) / 30) % 12) + 1;
          if (color_index === 12) color_index = 0;
          let color = this.colors[color_index];

          setTimeout(() => {
            if (color["hex"] === this.prediction) {
                this.state = 'win';
                this.win();
            } else {
                this.state = 'lose';
                this.lose();
            }
          }, 3500);
      },
      reset() {
        this.state = 'fresh';
        let loss_audio = document.getElementById("loss-audio");
        let win_audio = document.getElementById("win-audio");
        loss_audio.pause();
        loss_audio.currentTime = 0;
        win_audio.pause();
        win_audio.currentTime = 0;
      },
      lose() {
        let audio = document.getElementById("loss-audio");
        audio.play();
      },
      win() {
          let audio = document.getElementById("win-audio");
          audio.play();
      }
    }
}
</script>

<style lang="scss">

  $shadow: #222;

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

  button::-moz-focus-inner {
    border: 0;
  }

  button:hover {
      background: #008dd3;
  }

  button:focus {
      outline: none;
  }

  button.spin {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    width: 150px;
    height: 150px;
    border-radius: 100%;
    font-size: 24px;
  }

  .overlay {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0; bottom: 0; left: 0; right: 0;
    background: rgba(0, 0, 0, 0.8);
    z-index: 99;
    text-align: center;

    .win, .lose {
      position: absolute;
      top: 0; bottom: 0; left: 0; right: 0;
      margin: auto;
      width: 900px;
      height: 200px;
      text-align: center;
    }

    .text {
      font-size: 80px;
      font-weight: bold;
      
      .space {
        width: 15px;
      }
    }

    .win .text {
      span {
        color: gold;
        position: relative;
        top: 10px;
        display: inline-block;
        animation: bounce .3s ease infinite alternate;
        text-shadow: 0 1px 0 $shadow,
               0 2px 0 $shadow,
               0 3px 0 $shadow,
               0 4px 0 $shadow,
               0 5px 0 $shadow,
               0 6px 0 transparent,
               0 7px 0 transparent,
               0 8px 0 transparent,
               0 9px 0 transparent,
               0 10px 10px rgba(0, 0, 0, .4);
      }

      span:nth-child(2) { animation-delay: .1s; }
      span:nth-child(3) { animation-delay: .2s; }
      span:nth-child(4) { animation-delay: .3s; }
      span:nth-child(5) { animation-delay: .4s; }
      span:nth-child(6) { animation-delay: .5s; }
      span:nth-child(7) { animation-delay: .6s; }
      span:nth-child(8) { animation-delay: .7s; }
      span:nth-child(9) { animation-delay: .8s; }
      span:nth-child(10) { animation-delay: .9s; }
    }

    .lose .text {
      span {
        color: red;
        position: relative;
        top: 10px;
        display: inline-block;
        animation: bounce 1s ease 2 alternate;
        text-shadow: 0 1px 0 $shadow,
               0 2px 0 $shadow,
               0 3px 0 $shadow,
               0 4px 0 $shadow,
               0 5px 0 $shadow,
               0 6px 0 transparent,
               0 7px 0 transparent,
               0 8px 0 transparent,
               0 9px 0 transparent,
               0 10px 10px rgba(0, 0, 0, .4);
      }

      span:nth-child(2) { animation-delay: .1s; }
      span:nth-child(3) { animation-delay: .2s; }
      span:nth-child(4) { animation-delay: .3s; }
      span:nth-child(5) { animation-delay: .4s; }
      span:nth-child(6) { animation-delay: .5s; }
      span:nth-child(7) { animation-delay: .6s; }
      span:nth-child(8) { animation-delay: .7s; }
      span:nth-child(9) { animation-delay: .8s; }
      span:nth-child(10) { animation-delay: .9s; }
      span:nth-child(11) { animation-delay: .9s; }
      span:nth-child(12) { animation-delay: .9s; }
    }

    button {
      display: block;
      margin: 50px auto 0 auto;
      width: 200px;
    }
  }

  @keyframes bounce {
    100% {
      top: -10px;
      text-shadow: 0 1px 0 $shadow,
                   0 2px 0 $shadow,
                   0 3px 0 $shadow,
                   0 4px 0 $shadow,
                   0 5px 0 $shadow,
                   0 6px 0 $shadow,
                   0 7px 0 $shadow,
                   0 8px 0 $shadow,
                   0 9px 0 $shadow,
                   0 50px 25px rgba(0, 0, 0, .2);
    }
  }
</style>
