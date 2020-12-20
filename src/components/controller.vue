<template>
  <div>
    <div class="Indicator">
      <div :style="'width: ' + (currentTime / duration) * 100 + '%;'"></div>
      <input
        type="Range"
        step="0.000001"
        :max="duration"
        @input="liveTime"
        :value="currentTime"
      />
    </div>
    <span>
      <span>
        {{ maxMin &lt; 10 ? "0" + maxMin : maxMin }}:{{
          maxSec &lt; 10 ? "0" + maxSec : maxSec
        }}
      </span>
      /
      <span>
        {{ min &lt; 10 ? "0" + min : min }}:{{ sec &lt; 10 ? "0" + sec : sec }}
      </span>
    </span>

    <div class="controls">
      <div @click="play" :title="paused ? 'Pause' : 'Play'">
        <i v-if="paused" class="fas fa-pause"></i>
        <i v-else class="fas fa-play"></i>
      </div>

      <div class="Volume">
        <div title="Volume Plus" @click="volumeChange(1)">
          <i class="fas fa-plus"></i>
        </div>
        <div title="Mute" :class="mute ? 'on' : ''" @click="muted">
          <i class="fas fa-volume-slash"></i>
        </div>
        <div title="Volume Minus" @click="volumeChange(-1)">
          <i class="fas fa-minus"></i>
        </div>
      </div>

      <div>
        <div title="-5 Sec" @click="Time(-5)">
          <i class="fas fa-angle-double-left"></i>
        </div>
        <div title="+5 Sec" @click="Time(5)">
          <i class="fas fa-angle-double-right"></i>
        </div>
      </div>

      <div title="Repeat" :class="repeat ? 'on' : ''" @click="repeatChange">
        <i class="fas fa-repeat"></i>
      </div>

      <div title="Full Screen" @click="fullScreen">
        <i class="fal fa-arrows"></i>
      </div>

      <div title="information" @click="info = !info" style="position: relative">
        <i class="fas fa-info"></i>

        <ul class="info" v-if="info">
          <li v-for="information in informations" :key="information.key">
            <div class="key">{{ information.key }}</div>
            <div class="value">{{ information.value }}</div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  name: "Controller",
  props: [
    "paused",
    "fullscreen",
    "mute",
    "volume",
    "repeat",
    "currentTime",
    "duration",
    "speed"
  ],

  data() {
    return {
      sec: 0,
      min: 0,
      maxSec: 0,
      maxMin: 0,

      info: false,
      informations: [
        {
          key: "space",
          value: "Play - Pause"
        },
        {
          key: "F",
          value: "Full Screen"
        },
        {
          key: "R",
          value: "Repeat"
        },
        {
          key: "M",
          value: "Mute"
        },
        {
          key: "I",
          value: "Informations"
        },
        {
          key: "↑ Up",
          value: "Volume +"
        },
        {
          key: "↓ Down",
          value: "Volume -"
        },
        {
          key: "← Left",
          value: "-5 Secounds"
        },
        {
          key: "→ Right",
          value: "+5 Secounds"
        },
        {
          key: "⇑+.=[>]",
          value: "Speed +"
        },
        {
          key: "⇑+,=[<]",
          value: "Speed -"
        }
      ]
    };
  },

  methods: {
    fullScreen() {
      this.$emit("changeFullScreen", !this.fullscreen);
    },
    play(e) {
      this.$emit("changePaused", !this.paused);
      if (e) {
        this.$emit("setTranstion", e.target.innerHTML);
      }
    },
    muted(e) {
      if (e) {
        this.$emit("setTranstion", e.target.innerHTML);
      }
      this.$emit("changeMute", !this.mute);
    },
    repeatChange(e) {
      this.$emit("repeatChange", !this.repeat);
      this.$emit("setTranstion", e.target.innerHTML);
    },
    volumeChange(e) {
      this.$emit("volumeChange", e);
      this.$emit("changeMute", false);
    },
    Time(e) {
      this.$emit("changeTime", e);
    },
    liveTime: function(e) {
      this.$emit("liveTime", e.target.value);
    }
  },

  watch: {
    currentTime: function() {
      this.sec = Math.floor(this.currentTime % 60);
      this.min = Math.floor(this.currentTime / 60);
    },
    duration: function() {
      this.maxSec = Math.floor(this.duration % 60);
      this.maxMin = Math.floor(this.duration / 60);
    },
    speed: function() {
      this.$emit("setTranstion", this.speed + "x");
    }
  },

  created() {
    let that = this;
    onkeydown = function(e) {
      if (e.keyCode == 32) {
        // Space
        e.preventDefault();
        that.play();
        that.$emit("setTranstion", document.querySelector("[title]").innerHTML); // (Play-pause) Element
      }
      if (e.keyCode == 37) {
        // Left
        e.preventDefault();
        that.Time(-5);
      }
      if (e.keyCode == 38) {
        // Up
        e.preventDefault();
        that.volumeChange(1);
      }
      if (e.keyCode == 39) {
        // Right
        e.preventDefault();
        that.Time(5);
      }
      if (e.keyCode == 40) {
        // Down
        e.preventDefault();
        that.volumeChange(-1);
      }
      if (e.keyCode == 77) {
        // M key
        e.preventDefault();
        that.muted();
        that.$emit(
          "setTranstion",
          document.querySelector("[title = Mute]").innerHTML
        );
      }
      if (e.keyCode == 70) {
        // F key
        e.preventDefault();
        that.fullScreen();
      }
      if (e.keyCode == 73) {
        // I key
        e.preventDefault();
        that.info = !that.info;
      }
      if (e.keyCode == 82) {
        // R key
        e.preventDefault();
        that.$emit("repeatChange", !that.repeat);
        that.$emit(
          "setTranstion",
          document.querySelector("[title = Repeat]").innerHTML
        );
      }
      if (e.keyCode == 190 && e.shiftKey) {
        // > key
        e.preventDefault();
        that.$emit("speedChange", 0.25);
      }
      if (e.keyCode == 188 && e.shiftKey) {
        // < key
        e.preventDefault();
        that.$emit("speedChange", -0.25);
      }
    };
  }
});
</script>
<style lang="scss" scoped>
.controls div {
  cursor: pointer;
}

body .body .controls {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
}

body .body .controls i {
  width: 50px;
  height: 50px;
  border-radius: 35px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  pointer-events: none;
}

body .body .controls i::after {
  border-radius: 35px;
  content: "";
  width: 0;
  height: 0;
  position: absolute;
  background-image: radial-gradient(
    transparent,
    transparent,
    #dbd6d6,
    transparent
  );
  display: block;
  z-index: -1;
}

body .body .controls div:active > i::after {
  width: 100%;
  height: 100%;
  transition: 0.1s;
}

body .body .controls > div {
  border-radius: 35px;
  margin: 5px 10px;
  box-shadow: 0 7px 9px #dbd6d6, inset 0 3px 7px #fff;
  display: flex;
}

body .body .controls div.on {
  color: #ecad17;
}

body .body .controls .info .value {
  text-align: left;
  padding: 0 11px;
  flex: 1;
}

body .body .controls .info .key {
  border-right: 1px solid #eee;
  /*	text-align: center;*/
  width: 60px;
}

body .body .controls .info::after {
  content: "";
  width: 10px;
  height: 10px;
  background: rgb(41, 41, 41);
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translate(-50%, 50%) rotate(45deg);
  display: block;
}

body .body .controls .info {
  padding: 10px;
  border: 1px solid black;
  position: absolute;
  background: rgb(41, 41, 41);
  border-radius: 5px;
  bottom: 50px;
  width: 180px;
  font-family: monospace;
  left: 50%;
  color: white;
  transform: translateX(-50%);
  z-index: 1;
  box-shadow: 3px 0px 2px rgba(38, 38, 38, 0.84);
}

body .body .controls .info li {
  display: flex;
  margin: 3px 0;
  border-bottom: 1px solid #333;
}

body .body .Indicator {
  margin: 5px auto 7px;
  z-index: 1;
  box-sizing: border-box;
  background: #dbd6d6;
  position: relative;
  transition: 0.2s;
  overflow: hidden;
}

body .body .Indicator input {
  display: block;
  width: 100%;
  height: 10px;
  position: absolute;
  top: 0;
  z-index: 2;
}

body .body .Indicator div {
  position: relative;
  height: 3px;
  width: 0;
  background: #868686;
  transition: 0.2s;
  transition-property: height;
  z-index: 1;
}

body .body .Indicator:hover {
  margin-bottom: 0;
}

body .body .Indicator:hover div {
  height: 10px;
}

body .body input {
  z-index: 2;
  opacity: 0;
}
</style>
