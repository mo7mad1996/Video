<template>
  <div>
    <VideoElement
      :paused="paused"
      @changePaused="play($event)"
      :fullscreen="fullscreen"
      @changeFullScreen="changeFullScreen($event)"
      :mute="mute"
      :volume="volume"
      :currentTime="currentTime"
      @liveTime="liveTime($event)"
      :repeat="repeat"
      @endTime="endTime"
      :speed="speed"
      :transtion="transtion"
      @setTranstion="setTranstion($event)"
      :content="content"
    ></VideoElement>

    <Controller
      :paused="paused"
      @changePaused="play($event)"
      :fullscreen="fullscreen"
      @changeFullScreen="changeFullScreen($event)"
      :mute="mute"
      @changeMute="muted($event)"
      :repeat="repeat"
      @repeatChange="repeatChange($event)"
      :volume="volume"
      @volumeChange="volumeChange($event)"
      :currentTime="currentTime"
      @changeTime="changeTime($event)"
      @liveTime="liveTime($event)"
      :duration="duration"
      @setTranstion="setTranstion($event)"
      :speed="speed"
      @speedChange="speedChange($event)"
    ></Controller>

    <NavComponent :content="content"></NavComponent>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import VideoElement from "@/components/videoElement";
import Controller from "@/components/controller";
import NavComponent from "@/components/NavComponent";
// import axios from 'axios';

import json from "@/Json/data.json";

export default Vue.extend({
  name: "App",
  components: {
    VideoElement,
    Controller,
    NavComponent
  },
  data() {
    return {
      paused: false,
      fullscreen: false,
      mute: false,
      repeat: false,
      volume: 1,
      currentTime: 0,
      duration: 0,
      transtion: "",
      speed: 1,
      content: json
    };
  },
  methods: {
    play(value) {
      this.paused = value;
    },
    changeFullScreen(value) {
      this.fullscreen = value;
    },
    muted(value) {
      this.mute = value;
    },
    repeatChange(value) {
      this.repeat = value;
    },
    volumeChange(value) {
      if ((this.volume < 1 && value > 0) || (this.volume > 0 && value < 0)) {
        this.volume = (Math.floor(this.volume * 10) + value) / 10;
      }
      this.transtion = this.volume * 100 + "%";
    },
    changeTime(value) {
      this.currentTime += value;
      this.transtion = value + " sec";
    },
    liveTime(value) {
      this.currentTime = value;
    },
    endTime(value) {
      this.duration = value;
    },
    setTranstion(value) {
      this.transtion = value;
    },
    speedChange(value) {
      this.speed += value;
    }
  }
  // async created() {
  //   try {
  //     let res = await axios(json);

  //     console.log(res);
  //   } catch (err) {
  //     console.error(err);
  //   }
  // }
});
</script>
<style lang="scss" scoped>
div {
  position: relative;
}
</style>
