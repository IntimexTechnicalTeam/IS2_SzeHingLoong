<template>
  <div class="liveBox mobileVersion" style="text-align: center;">
    <div class="liveBox_in">
        <div class="TitleBg"><div class="innerBox">{{AboutContent.Title}}</div></div>
        <div class="videoBg">
            <p v-html="AboutContent.Body"></p>
        </div>
        <div class="btn">
          <router-link to="/cms/content/20295">{{$t('Cms.More')}}</router-link>
        </div>
    </div>
  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component
export default class HkAbout extends Vue {
  AboutContent:string='';
  getAboutContent () {
    this.$Api.cms.getContentByDevice({ Key: 'aboutus', IsMobile: true }).then(result => {
      this.AboutContent = result.CMS;
    });
  }
  handleScroll () {
    let fadeInElements = document.getElementsByClassName('fade-in');
    // console.log(document.getElementsByClassName('fade-in'), '滚动');
    for (var i = 0; i < fadeInElements.length; i++) {
      let elem = fadeInElements[i] as HTMLElement;
      if (this.isElemVisible(elem)) {
        elem.style.opacity = '1';
        // elem.style.transform = 'translate(0, 0)';
        // fadeInElements.splice(i, 1); // 只让它运行一次
      }
    }
    // document.removeEventListener('scroll', this.handleScroll);
  }
  isElemVisible (el) {
    var rect = el.getBoundingClientRect();
    var elemTop = rect.top + 100; // 200 = buffer
    var elemBottom = rect.bottom;
    return elemTop < window.innerHeight && elemBottom >= 0;
  }
  created () {
    this.getAboutContent();
    // document.addEventListener('scroll', this.handleScroll);
  }
  destroyed () {
    // document.removeEventListener('scroll', this.handleScroll);
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
  width: 100%;
  // height: 4.5rem;
  // background: url('/images/mobile/Mobile-back.png') center no-repeat;
  background-size: contain;
  text-align: center;
  position: relative;
  .innerBox{
    font-size: 2rem;
    color: #aa1638;
    // text-transform: uppercase;
    letter-spacing: 1px;
    padding-top: 2rem;
    font-weight: bold;
  }
}
.liveBox {
    width: 100%;
    // padding-bottom: 2rem;
    .liveBox_in{
    width: 90%;
    margin: 0 auto;
    margin-top: 2rem;
    margin-bottom: 2rem;
    padding-top:0;
    padding-bottom: 2rem;
    background-color: #fff;
    border-radius: 6px;
    box-shadow: 0 0 5px #707070;
      // .mapBg{
        // background: url('/images/mobile/mobileIndex_27.png') center center no-repeat;
        // background-size: 100% 100%;
        // display: inline-block;
        // padding: 2.5rem;
        // box-sizing: border-box;
      // }
      .videoBg{
        // background-size: 100% 100%;
        // display: inline-block;
        padding: 2rem 1rem;
        box-sizing: border-box;
        background: url('/images/mobile/indexbg_logo.png') center center no-repeat;
        // height: 6rem;
        margin: 0 auto;
        background-size: 80%;
        background-position: center;
        // height: 22rem;
        margin-bottom: 2rem;
        overflow: hidden;
        img{
          width: 100%;
        }
        /deep/ p{
          font-size: 1.2rem !important;
          line-height: 2rem;
          overflow: hidden;
          display: -webkit-box;
          -webkit-line-clamp: 10;
          -webkit-box-orient: vertical;
          word-break: break-word;
          color: #666666;
          text-align: justify;
          span{
            font-size: 1.2rem !important;
            color: #666666;
          }
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
      .btn{
        width: 12rem;
        height: 3rem;
        margin: 0 auto;
        text-align: center;
        // background: url(/images/mobile/cms_btn.png) center no-repeat;
        background-color: #aa1638;
        background-size: cover;
        box-sizing: border-box;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 3px;
        a{
          line-height: 3rem;
          font-size: 1.2rem;
          color: #fff;
          letter-spacing: 2px;
          text-transform: uppercase;
        }
      }
    }
    .fade-in {
      opacity: 0;
      transition: 0.5s all ease-out;
      // transform: translate(0, 30px);
      box-sizing: border-box;
      display: inline-block;
    }
}
</style>
