<template>
  <div id="home">
    <div class="page-tilte">
      <div class="insta-icon">
        <svg
          version="1.1"
          id="Layer_1"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          x="0px"
          y="0px"
          viewBox="0 0 512 512"
          style="enable-background:new 0 0 512 512;"
          xml:space="preserve"
        >
          <g>
            <g>
              <path
                d="M437.019,74.981C388.667,26.628,324.379,0,256,0S123.333,26.628,74.981,74.981C26.628,123.333,0,187.621,0,256
			s26.628,132.667,74.981,181.019C123.333,485.372,187.621,512,256,512s132.667-26.628,181.019-74.981S512,324.379,512,256
			S485.372,123.333,437.019,74.981z M256,495.832C123.756,495.832,16.168,388.244,16.168,256S123.756,16.168,256,16.168
			S495.832,123.756,495.832,256S388.244,495.832,256,495.832z"
              />
            </g>
          </g>
          <g>
            <g>
              <path
                d="M347.845,97.011H164.155c-37.024,0-67.144,30.121-67.144,67.144v183.692c0,37.022,30.121,67.143,67.144,67.143h183.692
			c37.022,0,67.143-30.121,67.143-67.144V164.155C414.989,127.131,384.869,97.011,347.845,97.011z M398.821,347.845
			c0,28.108-22.868,50.976-50.976,50.976H164.155c-28.108,0-50.976-22.868-50.976-50.976V164.155
			c0-28.108,22.868-50.976,50.976-50.976h183.692c28.107,0,50.975,22.868,50.975,50.976V347.845z"
              />
            </g>
          </g>
          <g>
            <g>
              <path
                d="M339.402,251.22c-2.391-42.533-37.002-76.75-79.558-78.669c-49.108-2.214-89.535,38.232-87.292,87.346
			c1.945,42.56,36.19,77.154,78.728,79.51c17.951,0.995,34.762-3.727,48.803-12.494c4.374-2.731,5.087-8.814,1.441-12.459
			c-0.039-0.039-0.077-0.077-0.115-0.115c-2.657-2.658-6.778-3.059-9.971-1.075c-10.491,6.519-22.892,10.241-36.158,10.102
			c-37.455-0.394-67.676-31.844-66.621-69.286c1.061-37.681,33.215-67.721,71.657-65.312c33.126,2.076,60.09,28.49,62.819,61.569
			c1.111,13.475-1.787,26.206-7.61,37.157c-1.667,3.136-1.153,6.982,1.358,9.493c0.041,0.041,0.083,0.083,0.124,0.124
			c3.773,3.773,10.154,2.886,12.675-1.816C336.664,282.269,340.301,267.197,339.402,251.22z"
              />
            </g>
          </g>
          <g>
            <g>
              <circle cx="342.232" cy="158.989" r="21.558" />
            </g>
          </g>
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
          <g />
        </svg>
      </div>
      <h1>InstaCard Maker</h1>
      <p>Developed by <a href="https://www.facebook.com/dang.nhz" target="_blank">Danny Ngo</a></p>
      <p>Inspired by <a href="https://www.facebook.com/groups/jbrteam" target="_blank">JBR-Team</a></p>
    </div>
    <div class="form-input">
      <label for="url-input"></label>
      <input
        v-model.trim="postUrl"
        @input="onChange"
        class="url-input"
        placeholder="Paste your instagram photo's URL here..."
      />
      <button
        :class="{'btn-disabled': disabled}"
        class="flat-btn"
        @click="generateCard"
        :disabled="disabled"
      >
        <span v-if="loading" class="loading-icon"></span>
        <span>Create</span>
      </button>
    </div>
    <HiddenCard :user="user"></HiddenCard>
    <LightBox v-if="showLightBox" :imageUrl="resultImage" @close-lightbox="onCloseLightBox"></LightBox>

    <div id="result">
      <div id="final-image" @click="onShowLightBox"></div>
      <div id="download-button"></div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import html2canvas from "html2canvas";
import * as StackBlur from "stackblur-canvas";
import HiddenCard from "./HiddenCard";
import LightBox from "./LightBox";
export default {
  name: "home",
  components: {
    HiddenCard,
    LightBox
  },
  data() {
    return {
      postUrl: "",
      loading: false,
      disabled: true,
      resultImage: null,
      showLightBox: false,
      user: null,
      imageUrl: null
    };
  },
  methods: {
    onShowLightBox() {
      this.showLightBox = true;
    },
    onCloseLightBox() {
      this.showLightBox = false;
    },
    onChange() {
      if (this.postUrl !== "") this.disabled = false;
      else this.disabled = true;
    },
   async generateCard() {
      document.getElementById("final-image").innerHTML = "";
      document.getElementById("download-button").innerHTML = "";
      let apiUrl = this.getApiUrl(this.postUrl);
      
      if(!apiUrl) return
      try {
        this.loading = true;
        this.disabled = true;
         const response = await axios.get(apiUrl)
         if(response) {
           const username = response.data.graphql.shortcode_media.owner.username
          await this.getUserInfo(username);
          this.imageUrl = response.data.graphql.shortcode_media.display_url
          const blurCanvas = document.getElementById("blur-background-canvas");
          const mainCanvas = document.getElementById("main-image-canvas");
          let image = new Image();
          image.setAttribute("crossOrigin", "anonymous");
          image.src = this.imageUrl;
          image.onload = async() => {
          this.drawImageScaled(image, blurCanvas);
          this.drawBlurImage(blurCanvas);
          this.drawImageScaled(image, mainCanvas);
          this.drawFinalCard()
          this.loading = false;
          this.postUrl = "";
      };
         }
      }catch (err) {
        alert(err)
      }
    },
    getApiUrl(postUrl) {
      if (postUrl.length !=0) {
        try {
          let postId = postUrl.split('/p/')[1].split('/')[0]
          let apiUrl = `https://www.instagram.com/p/${postId}/?__a=1`
          return apiUrl
        } catch (err){
          this.postUrl=''
          alert('Your URL is not correct!')
        }
      } else {
        this.postUrl=''
        alert('Your URL is not correct!')
      }
    },
    async getUserInfo(username) {
      if (username.length == 0) return
      try {
        const response = await axios.get(`https://www.instagram.com/${username}/?__a=1`)
        if (response) {
          this.user = response.data.graphql.user
        }
      }catch (err) {
        console.log(err)
      }
    }
    ,
    drawBlurImage(canvas) {
      StackBlur.canvasRGB(canvas, 0, 0, canvas.width, canvas.height, 20);
    },
    drawImageScaled(img, canvas) {
      let ctx = canvas.getContext("2d");
      let hRatio = canvas.width / img.width;
      let vRatio = canvas.height / img.height;
      let ratio = Math.max(hRatio, vRatio);
      let centerShift_x = (canvas.width - img.width * ratio) / 2;
      let centerShift_y = (canvas.height - img.height * ratio) / 2;
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      ctx.drawImage(
        img,
        0,
        0,
        img.width,
        img.height,
        centerShift_x,
        centerShift_y,
        img.width * ratio,
        img.height * ratio
      );
    },
    drawFinalCard() {
      html2canvas(document.querySelector("#wrapper"), {
        useCORS: true,
        allowTaint: true,
        scrollX: 0,
        scrollY: 0,
        width: 1200,
        height: 1200,
        scale: 1
      }).then(canvas => {
        const downloadButton = document.getElementById("download-button");
        const finalImage = document.getElementById("final-image")
        let image = new Image();
        image.src = canvas.toDataURL("image/jpeg", 1);
        this.resultImage = image.src;
        image.setAttribute("class", "result-image");
        finalImage.appendChild(image);
        // create download button
        //  let download = `<a id='download-btn' href='${image.src}' download='insta-card.jpg'>Download</a>`
        //  result.innerHTML += download 
        let download = document.createElement("a")
        download.setAttribute("id", "download-btn")
        download.setAttribute("href", image.src)
        download.setAttribute("download", "insta-card.jpg")
        download.innerText= "download"
        downloadButton.appendChild(download)
        
      });
    }
  }
};
</script>

<style lang="less">
@import '../assets/style/setting.less';
#home {
  width: 100%;
  background: white;
  .page-tilte {
    padding: 2rem 0;
    margin-bottom: 5rem;
    width: 100%;
    color: white;
    background: rgba(248, 14, 92, 1);
    .insta-icon{
      margin: 0 auto;
      width: 4rem;
      height: 4rem;
      fill: white;
    }
    h1{
      margin-top: 1rem;
      font-size: 4rem;
      @media @mobile, @large-mobile {
        font-size: 2rem;
      }
    }
  }
  .form-input {
    width: 50%;
    margin: 0 auto;
    @media @mobile, @large-mobile, @tablet {
      width: 95%;
    }
  }
  .flat-btn {
    width: 200px;
    position: relative;
    font-size: 1rem;
    height: 3rem;
    letter-spacing: 4px;
    transition: all 200ms linear;
    text-transform: uppercase;
    border: none;
    text-shadow: 1px 1px 2px rgba(150, 150, 150, 0.36);
    color: white;
    background: rgba(240, 50, 82, 1);
    cursor: pointer;
    @media @mobile, @large-mobile {
      width: 95%;
      margin-top: 30px;
    }
    &:focus {
      outline: 0;
    }

  }

  .btn-disabled {
    cursor: not-allowed;
    background: rgba(240, 50, 82, 0.8);
    &::after {
      width: 100%;
      height: 100%;
      display: block;
      background: white;
      content: "";
      opacity: 0.4;
      position: absolute;
      top: 0;
      left: 0;
    }
  }

  .loading-icon {
    display: inline-block;
    width: 15px;
    height: 15px;
    border-radius: 50%;
    border: 2px solid white;
    border-top-color: transparent;
    border-right-color: transparent;
    margin-right: 1rem;

    animation: spin 1s infinite linear;
    @keyframes spin {
      100% {
        transform: rotate(1turn);
      }
    }
  }

  .url-input {
    padding: 0.5rem 1rem;
    font-size: 1rem;
    width: calc(100% - 200px);
    height: 3rem;
    &:focus {
      outline: 0;
    }
    @media @mobile, @large-mobile{
      width: 95%;
    }
  }
  #result {
    margin: 5rem auto;
    position: relative;
    z-index: 999;
    width: 50%;
    cursor: zoom-in;
    @media @mobile, @large-mobile, @tablet {
      width: 95%;
    }
    .result-image {
      width: 100%;
      height: 100%;
      // border: 1px solid white;
    }
    #download-btn {
      display: block;
      text-transform: uppercase;
      padding: 0.5rem 0;
      height: fit-content;
      width: 150px;
      margin: 2rem auto;
      letter-spacing: 3px;
      transition: all 0.8s !important;
      background: white;
      color: black;
      border: 1px solid rgb(185, 185, 185);
      &:hover {
        border: 1px solid rgb(0, 140, 255);
        color: rgb(0, 140, 255);
      }
    }
  }
}
</style>