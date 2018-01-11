<template>
  <div class="camera-outer">
    <div class="camera-main" :style="style">
      <div class="camera-control">
        <button class="camera-btn" @click="cameraOn" v-show="!cameraon">打开摄像头</button>
        <button class="camera-btn" @click="cameraOff" v-if="cameraon">关闭摄像头</button>
        <button class="camera-btn" @click="toCanvas('video')" v-if="cameraon">生成Canvas</button>
      </div>
      <div class="camera-placement" @click="cameraOn" slot="placement" v-show="!cameraon">
        <p class="camera-placement-inner">
          {{msg}}
        </p>
      </div>
      <video class="camera-video" v-show="cameraon" :style="style"></video>
    </div>
    <div class="canvas-main" :style="style">
      <canvas class="camera-canvas" :width="ow" :height="oh" :style="style"></canvas>
      <div class="camera-canvas-group">
        <div class="camera-canvas-item" v-for="item in canvasGroup" @click="previewCanvas">
          <img :src="item" />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
var getUserMedia = require('getusermedia');
export default {
  props: {
    ow: {
      type: [Number, String],
      default: 400
    },
    oh: {
      type: [Number, String],
      default: 300
    },
    fillColor: {
      type: String,
      default: '#000'
    },
    placement: {
      type: String,
      default: '请点击打开摄像头'
    },
    maxView: {
      type: Number,
      default: 10
    }
  },
  data() {
    return {
      cameraon: false,
      videoEl: null,
      msg: this.placement,
      canvasGroup: [],
    }
  },
  computed: {
    style() {
      return { 'width': `${this.ow}px`, height: `${this.oh}px`, 'background-color': this.fillColor }
    }
  },
  mounted() {
    this.videoEl = this.$el.querySelector('video');
    this.canvasEl = this.$el.querySelector('canvas');
    this.context = this.canvasEl.getContext('2d');
  },
  methods: {
    cameraOn() {
      getUserMedia((err, stream) => {
        if (err) {
          console.log(err)
          this.msg = err.name;
          this.$emit('error', err);
        } else {
          this.cameraon = true;
          this.stream = stream;
          if (this.videoEl.mozSrcObject !== undefined) { // FF18a
            this.videoEl.mozSrcObject = stream;
          } else if (this.videoEl.srcObject !== undefined) {
            this.videoEl.srcObject = stream;
          } else { // FF16a, 17a
            this.videoEl.src = stream;
          }
        }
      });
    },
    cameraOff() {
      if (this.stream) {
        this.cameraon = false;
        this.msg = this.placement;
        this.context.fillStyle = "#000";
        this.context.fillRect(0, 0, this.ow, this.oh);
        this.stream.getVideoTracks()[0].stop();
        this.canvasGroup = [];
      }
    },
    previewCanvas(el) {
      this.context.fillStyle = "#000";
      this.context.fillRect(0, 0, this.ow, this.oh);
      this.context.drawImage(el.target || el, 0, 0, this.ow, this.oh);
      return this.canvasEl.toDataURL();
    },
    toCanvas() {
      const dataUrl = this.previewCanvas(this.videoEl);
      this.$emit('tocanvas', dataUrl);
      if (this.maxView == 0) {
        return;
      }
      if (this.canvasGroup.length + 1 > this.maxView) {
        this.canvasGroup.pop();
      }
      this.canvasGroup.unshift(this.canvasEl.toDataURL());
    }

  }
}

</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.camera-main {
  margin-right: 2px;
}

.camera-main,
.canvas-main {
  position: relative;
  float: left;
}

.camera-main video,
.camera-main .camera-placement {
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  z-index: 1;
}

.camera-placement {
  display: table;
}

.camera-placement-inner {
  display: table-cell;
  color: #fff;
  font-size: 18px;
  vertical-align: middle;
  text-align: center;
}

.camera-control {
  position: absolute;
  z-index: 10;
  left: 0;
  right: 0;
  bottom: 0;
  text-align: center;
  padding: 10px;
  min-height: 40px;
}

.camera-main:after {
  position: absolute;
  content: '';
  display: block;
  width: 100%;
  height: 50%;
  z-index: 9;
  left: 0;
  bottom: 0;
  max-height: 100px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0), rgba(0, 0, 0, 0.8));
}

.camera-btn {
  cursor: pointer;
  border: none;
  color: #fff;
  padding: 8px 12px;
  font-size: 14px;
  outline: none;
  background-color: rgba(255, 255, 255, 0.3);
  transition: all 0.3s;
  border: none;
  box-shadow: 0 2px 0 0 rgba(45, 140, 240, 0.8);
  margin: 0 5px;
}

.camera-btn:hover {
  background-color: rgba(45, 140, 240, 0.6);
  box-shadow: 0 2px 0 0 rgba(45, 140, 240, 1);
}

.camera-btn:active {
  background-color: rgba(45, 140, 240, 0.2);
}

.camera-canvas-group {
  display: flex;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
}

.camera-canvas-item {
  cursor: pointer;
  opacity: 0.8;
  flex: 1;
  max-width: 100px;
  height: 100%;
  overflow: hidden;
  clear: both;
  margin-bottom: -1px;
  transition: all 0.2s;
}

.camera-canvas-item img {
  float: left;
  width: 100%;
  background-color: #000;
  border: 1px solid #fff;
}

.camera-canvas-item:hover {
  position: relative;
  opacity: 1;
  position: relative;
  box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.8);
}

.camera-canvas-item:hover {}
.camera-outer:after{
  content: '';
  display: block;
  clear: both;
}
</style>
