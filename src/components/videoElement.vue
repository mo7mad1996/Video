<template>
  <div>
    <transition name="fade">
      <div class="fade" v-if="show" v-html="inner" @transitionend="end"></div>
    </transition>
    <video
      :poster="content[this.$route.params.id].img"
      :autoplay="paused"
      :src="content[this.$route.params.id].url"
      class="video"
      ref="video"
      @click="click"
      @dblclick="dblclick"
      @timeupdate="Time"
    ></video>
    {{ setTitle() }}
  </div>
</template>
<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  name: "VideoElement",
  props: [
    "paused",
    "fullscreen",
    "mute",
    "volume",
    "repeat",
    "currentTime",
    "transtion",
    "speed",
    "content"
  ],

  data() {
    return {
      inner: "",
      show: false,
      src: ""
    };
  },
  methods: {
    click() {
      this.$emit("changePaused", !this.paused);
    },
    dblclick() {
      this.$emit("changeFullScreen", !this.fullscreen);
    },
    Time() {
      this.$emit("liveTime", this.$refs.video.currentTime);
    },
    endTime() {
      this.$emit("endTime", this.$refs.video.duration);
    },
    end() {
      this.show = false;
      this.inner = "";
      this.$emit("setTranstion", "");
    },
    setTitle() {
      document.title =
        "متحف الدحيح | " + this.content[this.$route.params.id].title;
    }
  },

  watch: {
    paused: function() {
      if (this.paused) {
        this.$refs.video.play();
      } else {
        this.$refs.video.pause();
      }
    },
    fullscreen: function() {
      if (document.fullscreenElement === null) {
        this.$refs.video.requestFullscreen();
      } else {
        document.exitFullscreen();
      }
    },
    mute: function() {
      this.$refs.video.muted = this.mute;
    },
    volume: function() {
      this.$refs.video.volume = this.volume;
    },
    repeat: function() {
      this.$refs.video.loop = this.repeat;
    },
    currentTime: function() {
      if (
        Math.floor(this.$refs.video.currentTime) != Math.floor(this.currentTime)
      ) {
        // if (
        //   this.$refs.video.currentTime > this.currentTime + 1 ||
        //   this.$refs.video.currentTime < this.currentTime - 1
        // ) {
        // if (this.$refs.video.currentTime != this.currentTime) {
        this.$refs.video.currentTime = this.currentTime;
      }
    },
    transtion: function() {
      if (this.transtion) {
        this.inner = this.transtion;
        this.show = true;
      }
    },
    speed: function() {
      this.$refs.video.playbackRate = this.speed;
    }
  },

  created() {
    var that = this;
    onload = function() {
      that.endTime();
    };
    if (this.$route.params.id == undefined) {
      this.$route.params.id = 0;
    }
  }
});
</script>
<style scoped lang="scss">
div {
  .fade {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: rgba(0, 0, 0, 0.65);
    color: white;
    font-size: 20px;
    padding: 10px 20px;
    font-family: fantasy;
    border-radius: 4px;
    border: 1px solid black;

    &.fade-enter-active {
      transition: 0.5s;
    }

    &.fade-enter,
    &.fade-leave-to {
      opacity: 0;
    }
  }
  .video {
    width: 100%;
    max-height: calc(100vh - 70px);
    box-sizing: border-box;
    display: block;
    margin: auto;
    padding: 4px;
    border-radius: 10px;
    outline: 2px solid #868686;
  }
}
</style>
