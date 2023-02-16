<template>
  <div class="liveBox mobileVersion fade-in-hot" style="text-align: center;">
    <div class="liveBox_in">
        <!-- <div class="TitleBg"><div class="innerBox">{{$t('Cms.WhatNews')}}</div></div> -->
        <div class="mapBg">
           <p v-html="fbContent.Body"></p>
        </div>
        <div class="videoBg">
            <p v-html="videoContent.Body"></p>
        </div>
    </div>
  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component
export default class PkLiveBox extends Vue {
  videoContent:string='';
  fbContent:string='';
  getVideoContent () {
    this.$Api.cms.getContentByDevice({ ContentId: 20294, IsMobile: true }).then(result => {
      this.videoContent = result.CMS;
    });
  }
  getFbContent () {
    this.$Api.cms.getContentByDevice({ ContentId: 20299, IsMobile: true }).then(result => {
      this.fbContent = result.CMS;
    });
  }
  LiveScroll () {
      let fadeInElements = document.getElementsByClassName('fade-in-hot');
      // console.log(document.getElementsByClassName('fade-in'), '滚动');
      for (var i = 0; i < fadeInElements.length; i++) {
        let elem = fadeInElements[i] as HTMLElement;
        if (this.isElemVisible(elem)) {
          elem.style.opacity = '1';
          elem.style.transform = 'translate(0, 0)';
          // fadeInElements.splice(i, 1); // 只让它运行一次
        }
      }
      // document.removeEventListener('scroll', this.handleScroll);
    }
    isElemVisible (el) {
      var rect = el.getBoundingClientRect();
      var elemTop = rect.top + 300; // 200 = buffer
      var elemBottom = rect.bottom;
      return elemTop < window.innerHeight && elemBottom >= 0;
    }
  mounted () {
    document.addEventListener('scroll', this.LiveScroll);
  }
  destroyed() {
     document.removeEventListener('scroll', this.LiveScroll);
  }
  created () {
    this.getVideoContent();
    this.getFbContent();
  }
  get lang () {
    return this.$Storage.get('locale');
  }
}
</script>
<style  lang="less">
.mobileVersion{
  .videoBg{
    iframe{
      width: 100%;
      height: 20rem;
    }
    img{
      width: 100%;
    }
  }
  .mapBg{
    img{
      width: 100%;
    }
  }
}
</style>
<style scoped lang="less">
/*live*/
.TitleBg{
  width: 90%;
  height: 5rem;
  border:1px solid #4d4d4d;
  margin: 0 auto;
  padding: .5rem;
  margin-bottom: 2rem;
  .innerBox{
    width: 100%;
    height: 100%;
    background:#4d4d4d;
    color: #FFF;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    font-weight: 700;
    font-family: 'Arial';
  }
}
.liveBox {
    width: 100%;
    padding-bottom: 2rem;
    // background: url('/images/mobile/Mobile-index_04.jpg') no-repeat center center;
    background-size: 100% 100%;
    .liveBox_in{
    width: 100%;
    margin: 0 auto;
    padding-top:0;
      // .mapBg{
        // background: url('/images/mobile/mobileIndex_27.png') center center no-repeat;
        // background-size: 100% 100%;
        // display: inline-block;
        // padding: 2.5rem;
        // box-sizing: border-box;
      // }
      .videoBg{
        margin-top: 2rem;
        margin-bottom: 2rem;
        background-size: 100% 100%;
        display: block;
        // padding: 2.5rem;
        box-sizing: border-box;
        width: 100%;
        img{
          width: 100%;
        }
      }
      .hotTitle{
        width: 100%;
        background: url('/images/mobile/mobileIndex_25.png') center 0 no-repeat;
        height: 6rem;
        margin: 0 auto;
        background-size: contain;
      }
      .hotTitleE{
        width: 100%;
        background: url('/images/pc/whatnews.png') center 0 no-repeat;
        height: 6rem;
        margin: 0 auto;
        background-size: contain;
      }
    }

}
.fade-in-hot {
  opacity: 0;
  transition: 1s all ease-out;
  // transform: translate(0, -30px);
  box-sizing: border-box;
}
</style>
