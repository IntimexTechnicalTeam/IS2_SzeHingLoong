<template>
  <div class="map-main liveBox fade-in" style="text-align: center;">
    <div class="liveBox_in">
        <div class="TitleBg"><div class="innerBox">{{AboutContent.Title}}</div></div>
        <div class="videoBg ">
            <p v-html="AboutContent.Body"></p>
        </div>
    </div>
  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component
export default class PkMap extends Vue {
  AboutContent:string='';
  getAboutContent () {
    this.$Api.cms.getContentByDevice({ Key: 'contactus', IsMobile: false }).then(result => {
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
    var elemTop = rect.top + 250; // 200 = buffer
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

<style scoped lang="less">
.TitleBg{
  width: 100%;
  // height: 79px;
  // background: url('/images/pc/titilebg.png') center no-repeat;
  background-size: contain;
  text-align: center;
  position: relative;
  margin-top: 50px;
  margin-bottom: 50px;
  .innerBox{
    font-size: 30px;
    color: #aa1638;
    // text-transform: uppercase;
    font-weight: bold;
    // letter-spacing: 1px;
    // padding-top: 44px;
  }
}
.map-main {
  width: 100%;
  margin: 0 auto;
  display: flex;
}
.map-main img {
  width: 100%;
}
.liveBox {
    width: 1200px;
    margin: 0 auto;
    padding-bottom: 0;
    .liveBox_in{
    width: 100%;
    margin: 0 auto;
    margin-bottom: 50px;
    // margin-top: 2rem;
    // margin-bottom: 100px;
    // padding-top:2rem;
    // padding-bottom: 2rem;
      .videoBg{
        // background-size: 100% 100%;
        // display: inline-block;
        // padding: 2rem 1rem;
        // margin: 2rem auto;
        margin-top: 44px;
        box-sizing: border-box;
        img{
          width: 100%;
        }
      }
    }

    /deep/ .contactBox{
      p{
        font-size: 18px;
        // margin-bottom: 2rem;
        text-align: center;
        // display: flex;
        // justify-content: center;
        // float: left;
        display: table-cell;
        width: 25%;
        iframe{
          border: none;
        }
        span.icon{
          display: flex;
          justify-content: center;
          align-items: center;
          font-size: 18px;
          color: #aa1638;
          font-family: 'Poppins-Regular';
          margin-bottom: 20px;
          img{
            // width: 2rem;
            height: 30px;
            display: block;
            margin-right: 10px;
          }
        }
        span{
          font-family: 'Poppins-Light';
          font-size: 18px !important;
          a{
            color: #333333;
            font-size: 18px !important;
            font-family: 'Poppins-Light';
          }
        }
      }
      .maps{
        margin-top: 72px;
        iframe{
          border: none;
        }
      }
    }
}
.fade-in {
  opacity: 0;
  transition: 0.5s all ease-out;
  // transform: translate(0, 30px);
  box-sizing: border-box;
  // display: inline-block;
}
</style>
