<template>
<div id="footer">
  <div class="footbg" :class="{footerMenus_Eng : $Storage.get('locale') === 'E'}">
    <div class="footerMain">
        <!-- <div class="footerTop">
            <p><span>whatsapp&nbsp;{{$t('home.Order')}}</span><b>6289 1789</b></p>
            <p><span>{{$t('home.TelSearch')}}</span><b>6289 1789</b></p>
        </div> -->
        <div class="footerBotttom">
          <div class="logoBox">
              <a href="/"><img class="logo1" src="/images/pc/logo2_JPG.png"> <img class="logo2" src="/images/pc/pcindex_09.png"></a>
          </div>
          <div class="footerLeft">
              <ul>
              <li
                v-for="(item, index) in $store.state.footerMenus"
                :key="index"
              >
                <a
                  href="javascript:;"
                  v-if="item.Type === -1"
                  @click="toUrl(item.Url)"
                >
                  {{ item.Name }}
                </a>
                <router-link v-else :to="To(item)" slot="title">
                  {{ item.Name }}
                </router-link>
                <!-- <ul>
                  <li v-for="(c, index2) in item.Childs" :key="index2">
                    <router-link :to="To(c)">{{ c.Name }}</router-link>
                  </li>
                </ul> -->
              </li>
            </ul>
          </div>
          <div class="footerRight">
            <p><a href="https://www.instagram.com/shl_hk/"><img src="/images/pc/ig.png" /></a></p>
            <p><a href="https://www.facebook.com/shlhk"><img src="/images/pc/fb.png" /></a></p>
            <p><a href="https://www.youtube.com/watch?v=jBksP6soTUc"><img src="/images/pc/youtube.png" /></a></p>
            <p><img src="/images/pc/stripe.png" /></p>
            <p><img src="/images/pc/wechatpay.png" /></p>
            <p><img src="/images/pc/alipay.png" /></p>
          </div>
          <div class="clear"></div>

        </div>
        <p class="footercopy">
              <span>Copyright {{currentYear}} © 時興隆(香港)有限公司 Powerd by Eventizer
                <a href="https://www.intimex.hk/" target="_blank">
                  <img src="/images/pc/footerlogo.png">
                </a>
              </span>
            </p>
    </div>
  </div>
</div>

</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component
export default class InsFooterLayout1 extends Vue {
  currentYear: number = 0;
  footerMenus: any[] = [];
  goToTop () {
    let sTop = document.documentElement.scrollTop;
    let times = setInterval(() => {
      sTop -= 50;
      if (sTop <= 0) {
        document.documentElement.scrollTop = 0;
        clearInterval(times);
      } else {
        document.documentElement.scrollTop = sTop;
      }
    }, 1);
  }
  To (n) {
    let url = '';
    if (n.Type === 0) {
      url = n.Url;
    } else if (n.Type === 1 && n.IsAnchor === false) {
      url = '/cms/catDetail/' + n.Value.Id;
    } else if (n.Type === 1 && n.IsAnchor === true) {
      url = '/CMS/catDetail/' + n.ParentId + '#' + n.Value.Id;
    } else if (n.Type === 2) {
      url = '/CMS/content/' + n.Value.Id;
    } else if (n.Type === 3) {
      url = '/RegNPay/Form/' + n.Value.Id;
    } else if (n.Type === 4 && !this.$store.state.catMenuType) {
      url = '/product/cat/' + n.Value.Id;
    } else if (n.Type === 4 && this.$store.state.catMenuType) {
      url =
        '/product/search/-?catalogs=' +
        JSON.stringify([parseInt(n.Value.Id)]) +
        '&type=0';
    } else if (n.Type === 5) {
      url =
        '/product/search/-?attrs=' +
        JSON.stringify([{ Id: parseInt(n.Value.Id), Vals: [] }]) +
        '&type=0';
    } else {
      url =
        '/product/search/-?attrs=' +
        JSON.stringify([
          { Id: parseInt(n.ParentId), Vals: [parseInt(n.Value.Id)] }
        ]) +
        '&type=0';
    }
    return url;
    // return n.Type === 1 ? '/cms/catDetail/' + n.Value.Id : n.Type === 2 ? '/CMS/content/' + n.Value.Id : n.Type === 3 ? '/RegNPay/Form/' + n.Value.Id : n.Type === 4 && !this.$store.state.catMenuType ? '/product/cat/' + n.Value.Id : n.Type === 4 && this.$store.state.catMenuType ? '/product/search/-?catalogs=' + JSON.stringify([parseInt(n.Value.Id)]) + '&type=0' : n.Type === 5 ? '/product/search/-?attrs=' + JSON.stringify([{ Id: parseInt(n.Value.Id), Vals: [] }]) + '&type=0' : '/product/search/-?attrs=' + JSON.stringify([{ Id: parseInt(n.ParentId), Vals: [parseInt(n.Value.Id)] }]) + '&type=0';
  }
  getMenu () {
    this.$Api.promotion.getMenu().then((result) => {
      this.footerMenus = result.ReturnValue.FooterMenus;
    });
  }
  created () {
    var date = new Date();
    this.currentYear = date.getFullYear();
    this.getMenu();
  }
}
</script>

<style scoped lang="less">
/* 底部文件 */
.footbg{
    // background: #9f2f34 url('/images/pc/pcindex_05.jpg') no-repeat center bottom;
    background-size: cover;
    width: 100%;
    display: inline-block;
    padding-bottom: 10px;
    // min-height: 278px;

}
.footerMain{
    width: 1200px;
    margin: 0 auto;
}
.footerTop{
    text-align: center;
    padding-top: 25px;
    padding-bottom: 25px;
    width: 100%;
}
.footerTop p{
    text-align: center;
    display: inline-block;
    margin-right: 50px;
}
.footerTop p span{
    font-size: 14px;
    color: #FFF;
    line-height: 35px;
    margin-right: 15px;
}
.footerTop p b{
    font-weight: 100;
    font-size: 35px;
    color: #FFF;
    line-height: 35px;
}
.footerBotttom{
    width: 100%;
    display: flex;
    align-content: center;
    padding-top: 10px;
}
.logoBox a{
    display: flex;
    align-items: center;
}
.logoBox a img.logo1{
   width: 61px;
   display: block;
   margin-right: 16px;
}
.logoBox a img.logo2{
   width: 222px;
   display: block;
}
.footerLeft{
    // float: left;
    width: 100%;
}
.footerLeft > ul{
    display: flex;
    width: 100%;
    height: 68px;
    align-items: center;
}
.footerLeft > ul >li{
    // width: 100%;
    // line-height: 30px;
    padding: 0 25px;
}
.footerLeft > ul >li >a{
    font-size:16px;
    color:#333;
    // padding: 0 25px;
}
.footerLeft > ul >li >ul{
  width: 100%;
}
.footerLeft > ul >li >ul a{
    font-size: 16px;
    color:#333;
    display: inline-block;
    text-transform: uppercase;
}
.footerLeft > ul >li >ul a:hover{
   transform: translateY(-3px);
}
.footerLeft p{
    width: 100%;
    display: block;
    font-size: 14px;
    color: #333;
    padding-top: 20px;
}
.footerLeft p img{
    display: inline-block;
    vertical-align: middle;
    padding-left: 10px;
}
.footerRight{
    float: right;
    // width: 40%;
    text-align: center;
    display: flex;
    align-items: center;
}
.footerRight p{
  margin: 0 5px;
}
.footerRight img{
  // width: 60%;
  display: block;
}
.footercopy{
  width: 100%;
  // display: inline-block;
  margin-top: 20px;
  text-align: center;
}
.footercopy span:nth-child(1){
  // float: left;
  color:#333;
  font-size: 14px;
}
.footercopy span:nth-child(1) img{
  display: inline-block;
  vertical-align:middle;
  padding-left: 10px;
}
.footercopy span:nth-child(2){
  float: right;
  width: 40%;
  text-align: center;
  color:#333;
  font-size: 14px;
}
.footercopy span:nth-child(2) img{
  display: inline-block;
  vertical-align:middle;
  padding-left: 10px;
}
.footerMenus_Eng{
      .footerLeft > ul > li{
        padding: 0 22px;
      }
    }
</style>
