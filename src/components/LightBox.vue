<template>
  <div id="light-box" v-if="showLightBox">
    <div class="btn-close" @click="closeLightBox">
        <div class="line-item"></div>
        <div class="line-item"></div>
    </div>
    <!-- display image here -->
    <div class="images-wrapper">
      <img id="image" @click="zoomImage" v-if="imageUrl" :src="imageUrl" alt="image" />
    </div>
  </div>
</template>

<script>
export default {
  name: "light-box",
  props: ["imageUrl"],
  data: function() {
    return {
      //   index: this.imageIndex
    };
  },
  methods: {
    closeLightBox() {
      this.$emit('close-lightbox')
    },
    zoomImage() {
      const image = document.querySelector("#image");
      if (image.style.width === "100%") {
        image.style.cursor = "zoom-in";
        image.style.width = "50%";
      } else {
        image.style.cursor = "zoom-out";
        image.style.width = "100%";
      }
    }
  },
  computed: {
    showLightBox() {
      return true
    }
  }
};
</script>
<style lang="less" scoped>
@import '../assets/style/setting.less';
#light-box {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  display: block;
  background: rgba(0, 0, 0, 0.8);
  z-index: 9999;
  overflow-y: scroll;
  overflow-x: hidden;
  &::-webkit-scrollbar {
    display: none;
  }

  .btn-close {
    color: #fff;
    cursor: pointer;
    position: fixed;
    top: 2rem;
    right: 2rem;
    opacity: 0.8;
    mix-blend-mode: difference;
    transition: 0.5s;
    &:hover {
      opacity: 1;
    }

    .line-item {
        display: block;
        content: '';
        width: 2rem;
        height: 3px;
        background: white;
        transform-origin: center center;
        
        &:nth-child(1) {
            transform: rotate(45deg) translate(0px,2px);
        }
        &:nth-child(2) {
            transform: rotate(-45deg) translate(0px,-3px);
        }
    }
  }

  .images-wrapper {
    background: #fff;
    //    margin-top: 2rem;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 2rem;
    width: 80%;
    min-height: 100vh;
    height: fit-content;
    margin: 2rem auto;
    @media @mobile, @large-mobile {
    width: 100%;
    margin: 0;
    padding: 0;
  }
    img {
      cursor: zoom-in;
      width: 50%;
      height: auto;
      @media @mobile, @large-mobile {
            width: 100%;
        }
    }
  }
}
</style>