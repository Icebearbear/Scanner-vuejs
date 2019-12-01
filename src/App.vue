<template>
  <div>
        <v-quagga :onDetected="logIt"  :readerSize="readerSize" :readerTypes="['ean_reader']"></v-quagga>
  </div>
</template>

<script>
import Quagga from 'vue-quaggajs';
import Vue from 'vue';

Vue.use(Quagga);

export default {
  name: 'app',

    data () {
    return {
      readerSize: {
        width: 640,
        height: 480
      },
      detecteds: []
    };
  },
  methods: {
    logIt (data) {
      console.log('detected', data);
      console.log(data.codeResult.code);
      console.log(Quagga.canvas); 
    },

    processed (data) {
  let drawingCtx = Quagga.canvas.ctx.overlay,
    drawingCanvas = Quagga.canvas.dom.overlay;
 
  if (data) {
    if (data.boxes) {
      drawingCtx.clearRect(0, 0, parseInt(drawingCanvas.getAttribute("width")), parseInt(drawingCanvas.getAttribute("height")));
      data.boxes.filter(function (box) {
        return box !== data.box;
      }).forEach(function (box) {
        Quagga.ImageDebug.drawPath(box, {x: 0, y: 1}, drawingCtx, {color: "green", lineWidth: 2});
      });
    }
    if (data.box) {
      Quagga.ImageDebug.drawPath(data.box, {x: 0, y: 1}, drawingCtx, {color: "#00F", lineWidth: 2});
    }
 
    if (data.codeResult && data.codeResult.code) {
      Quagga.ImageDebug.drawPath(data.line, {x: 'x', y: 'y'}, drawingCtx, {color: 'red', lineWidth: 3});
    }
  }
},

  }
 
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
