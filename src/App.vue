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

// videos
// import video1 from "@/Videos/1.mp4";
// import video2 from "@/Videos/2.mp4";
// import video3 from "@/Videos/3.mp4";

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
      content: [
        {
          title: "نابليون بونابرت",
          url:
            "https://s401d3.akwam.download/download/1607529904/5fcfa430c8ec5/El.Dahee7.S01E01.480P.WEB-DL.akwam.net.mp4",
          img:
            "https://shahidstatic1.akamaized.net/mediaObject/2020/disney/Odai/127_Hours/Daheeh_Ep1/original/"
        },
        {
          title: "ألبرت أينشتاين",
          url:
            "https://s401d3.akwam.download/download/1607529862/5fcfa40678f07/El.Dahee7.S01E02.480P.WEB-DL.akwam.net.mp4",
          img:
            "https://shahidstatic4.akamaized.net/mediaObject/2020/disney/Odai/127_Hours/Daheeh_Ep2_/original/"
        },
        {
          title: "الاسكندر الاكبر",
          url:
            "https://s402d1.akwam.download/download/1607529821/5fcfa3dd8e9aa/El.Dahee7.S01E03.480P.WEB-DL.akwam.net.mp4",
          img:
            "https://shahidstatic2.akamaized.net/mediaObject/2020/No-Tags/No-Tags-2/ep3_Clean_slider_Ar/original"
        },
        {
          title: "ستيف جوبز",
          url:
            "https://s402d1.akwam.download/download/1607529633/5fcfa3210551b/El.Dahee7.S01E04.480P.WEB-DL.akwam.net.mp4",
          img:
            "https://shahidstatic1.akamaized.net/mediaObject/2020/disney/Odai/Clean_Daheeh_Ep4/original"
        },
        {
          title: "كليوبترا",
          url:
            "https://s301d2.akwam.download/download/1607529516/5fcfa2ac459eb/El.Dahee7.S01E05.480P.WEB-DL.akwam.net.mp4",
          img:
            "https://shahidstatic3.akamaized.net/mediaObject/2020/disney/Odai/127_Hours/3000071689_Daheeh/original"
        }
      ]
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
});
</script>
<style lang="scss" scoped>
div {
  position: relative;
}
</style>
