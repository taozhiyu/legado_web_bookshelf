<template>
  <div v-bind:style="style">
    <template v-for="a in carray">
      <p v-if="!/<img[^>]*src/.test(a)" v-html="a" :key="a"></p>
      <img v-else v-lazy="getImageSrc(a)" :key="a + ''" />
    </template>
  </div>
</template>
<script>
import config from "../plugins/config";
export default {
  name: "pcontent",
  data() {
    return {
      style: this.getStyle,
    };
  },
  props: ["carray"],
  // render() {
  //   const { getStyle } = this;
  //   if (this.show) {
  //     return (
  //       <div>
  //         {this.carray.map((a) => {
  //           if (!/<img[^>]*src/.test(a)) {
  //             return <p style={getStyle} domPropsInnerHTML={a} />;
  //           }
  //           return <img v-lazy={this.getImageSrc(a)} />;
  //         })}
  //       </div>
  //     );
  //   } else {
  //     return <div />;
  //   }
  // },
  computed: {
    show() {
      return this.$store.state.showContent;
    },
    getStyle() {
      let style = { fontSize: this.fontSize };
      if (this.$store.state.config.font >= 0) {
        return Object.assign(
          config.fonts[this.$store.state.config.font],
          style
        );
      }
      style.fontFamily = this.$store.state.config.customFontName;
      return style;
    },
    fontSize() {
      return this.$store.state.config.fontSize + "px";
    },
  },
  methods: {
    getImageSrc(content) {
      let imgPattern = /<img[^>]*src="([^"]*(?:"[^>]+\})?)"[^>]*>/;
      return content.match(imgPattern)[1];
    },
  },
  watch: {
    fontSize() {
      let that = this;
      that.style = that.getStyle;
      that.$store.commit("setShowContent", false);
      this.$nextTick(() => {
        that.$store.commit("setShowContent", true);
      });
    },
  },
};
</script>

<style lang="scss" scoped>
p {
  display: block;
  word-wrap: break-word;
  word-break: break-all;
}
img {
  margin-left: auto;
  margin-right: auto;
  display: block;
  width: 100%;
}
</style>
