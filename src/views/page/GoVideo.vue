<template>
  <div class="content g2-content">
    <div class="video-content">
      <iframe
        width="100%"
        height="100%"
        :src="apiurl"
        frameborder="0"
        border="0"
        marginwidth="0"
        marginheight="0"
        scrolling="no"
        allowtransparency="true"
        allowfullscreen="true"
      ></iframe>
    </div>
    <div class="card">
      <header class="card-header">
        <p class="card-header-title">
          <span class="icon">
            <i class="fa fa-play-circle" aria-hidden="true"></i>
          </span>
          {{ $t("page.video.play") }} /
          <span class="icon">
            <i class="fa fa-download" aria-hidden="true"></i>
          </span>
          {{ $t("page.video.download") }}
        </p>
      </header>
      <div class="card-content">
        <div class="content">
          <div class="field">
            <label class="label">{{ $t("page.video.link") }}</label>
            <div class="control">
              <input class="input" type="text" :value="videourl" />
            </div>
          </div>
          <div class="columns is-mobile is-multiline has-text-centered">
            <div
              class="column"
              v-for="(item, index) in players"
              v-bind:key="index"
            >
              <p class="heading">
                <a :href="item.scheme">
                  <figure class="image is-48x48" style="margin: 0 auto;">
                    <img class="icon" :src="item.icon" />
                  </figure>
                </a>
              </p>
              <p class>{{ item.name }}</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { decode64 } from "@utils/AcrouUtil";
export default {
  data: function() {
    return {
      apiurl: "",
      videourl: "",
    };
  },
  methods: {
    render() {
      // 便于开发环境调试
      this.videourl = window.location.origin + encodeURI(this.url);
      this.apiurl =
        "https://api.jsonpop.cn/demo/blplyaer/?url=" + this.videourl;
    },
  },
  activated() {
    this.render();
  },
  computed: {
    url() {
      if (this.$route.params.path) {
        return decode64(this.$route.params.path);
      }
      return "";
    },
    players() {
      return [
        {
          name: "IINA",
          icon: "https://www.iina.io/images/iina-icon-60.png",
          scheme: "iina://weblink?url=" + this.videourl,
        },
        {
          name: "PotPlayer",
          icon: "https://cloud.jsonpop.cn/go2index/player/potplayer.png",
          scheme: "potplayer://" + this.videourl,
        },
        {
          name: "VLC",
          icon: "https://cloud.jsonpop.cn/go2index/player/vlc.png",
          scheme: "vlc://" + this.videourl,
        },
        {
          name: "Thunder",
          icon: "https://cloud.jsonpop.cn/go2index/player/thunder.png",
          scheme: "thunder://" + this.getThunder,
        },
        {
          name: "Aria2",
          icon: "https://cloud.jsonpop.cn/go2index/player/aria2.png",
          scheme: 'javascript:alert("ça peut marcher un jour?")',
        },
        {
          name: "nPlayer",
          icon: "https://cloud.jsonpop.cn/go2index/player/nplayer.png",
          scheme: "nplayer-" + this.videourl,
        },
        {
          name: "MXPlayer(Free)",
          icon: "https://www.iina.io/images/iina-icon-60.png",
          scheme:
            "intent:" +
            this.videourl +
            "#Intent;package=is.xyz.mpv;S.title=" +
            this.title +
            ";end",
        },
        {
          name: "MXPlayer(Pro)",
          icon: "https://cloud.jsonpop.cn/go2index/player/mxplayer.png",
          scheme:
            "intent:" +
            this.videourl +
            "#Intent;package=com.mxtech.videoplayer.pro;S.title=" +
            this.title +
            ";end",
        },
      ];
    },
    getThunder() {
      return Buffer.from("AA" + this.videourl + "ZZ").toString("base64");
    },
  },
};
</script>
