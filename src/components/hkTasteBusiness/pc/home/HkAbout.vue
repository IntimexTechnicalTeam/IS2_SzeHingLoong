<template>
  <div class="liveBox mobileVersion fade-in" style="text-align: center;">
    <div class="liveBox_in">
        <div class="TitleBg"><div class="innerBox">{{AboutContent.Title}}</div></div>
        <div class="videoBg">
            <div class="dody" v-html="AboutContent.Body"></div>
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
    this.$Api.cms.getContentByDevice({ Key: 'aboutus', IsMobile: false }).then(result => {
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
    document.addEventListener('scroll', this.handleScroll);
  }
  destroyed () {
    document.removeEventListener('scroll', this.handleScroll);
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
  // height: 79px;
  // background: url('/images/pc/titilebg.png') center no-repeat;
  background-size: contain;
  text-align: center;
  position: relative;
  .innerBox{
    font-size: 30px;
    color: #aa1638;
    // text-transform: uppercase;
    font-weight: bold;
    // letter-spacing: 1px;
    // padding-top: 44px;
  }
}
.liveBox {
    width: 100%;
    // padding-bottom: 2rem;
    .liveBox_in{
    width: 1200px;
    margin: 0 auto;
    // margin-top: 2rem;
    margin-bottom: 50px;
    padding-top:30px;
    padding-bottom: 30px;
    background-color: #fff;
    // border-radius: 6px;
    box-shadow: 0 0 5px #efeded;
    border-radius: 5px;
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
        padding: 25px 50px;
        box-sizing: border-box;
        // background: url('/images/pc/indexbg_logo.png') center center no-repeat;
        // height: 6rem;
        margin: 0 auto;
        background-size: 521px;
        background-position: center;
        // height: 22rem;
        margin-bottom: 36px;
        overflow: hidden;
        img{
          width: 100%;
        }
        /deep/ p{
          font-size: 16px !important;
          line-height: 30px;
          overflow: hidden;
          display: -webkit-box;
          -webkit-line-clamp: 10;
          -webkit-box-orient: vertical;
          word-break: break-word;
          color: #666666;
          text-align: left;
          font-family: 'Poppins-Light';
          span{
            font-size: 16px !important;
            color: #666666;
            font-family: 'Poppins-Light';
          }
        }
        .dody{
          // height: 270px;
          overflow: hidden;
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
        width: 140px;
        height: 46px;
        margin: 0 auto;
        text-align: center;
        background-color: #aa1638;
        // background: url(/images/pc/cms_btn.png) center no-repeat;
        background-size: cover;
        box-sizing: border-box;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 3px;
        a{
          line-height: 46px;
          font-size: 18px;
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
