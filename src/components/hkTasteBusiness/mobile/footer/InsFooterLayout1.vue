<template>
 <div id="footer">
    <div class="footer-box">
          <p><img class="logo1" src="/images/pc/logo2_JPG.png"> <img class="logo2" src="/images/pc/pcindex_09.png"></p>
         <!--  <div class="contactBox">
              <div class="w50">
                  <p>whatsapp&nbsp;{{$t('home.Order')}}</p>
                  <p>6289 1789</p>
              </div>
              <div class="w50">
                  <p>{{$t('home.TelSearch')}}</p>
                  <p>6289 1789</p>
              </div>
          </div> -->
          <div class="footerNav">
                  <ul>
          <li
            v-for="(item, index) in footerMenus"
            :key="index"
            class="indexMeun"
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
            <i
              :class="{ downIcon: item.showSub, upIcon: !item.showSub }"
              class="downIcon"
              @click="showMeun(item, index)"
              v-if="item.Childs && item.Childs.length > 0"
            ></i>
            <transition name="sub-comments">
              <ul
                v-show="item.showSub"
                v-if="item.Childs && item.Childs.length"
                :class="'SubMeun'+index"
              >
                <li v-for="(child, index2) in item.Childs" :key="index2">
                  <router-link :to="To(child)">{{ child.Name }}</router-link>
                  <!-- <router-link  @click.native="closeSlideMenu(item.Childs)" :to="To(item.Childs)" slot="title">
                                        <b>{{child.Name}}</b>
                                    </router-link> -->
                </li>
              </ul>
            </transition>
          </li>
        </ul>
              </div>
          <div class="footerAccept fix">
            <div class="left">
              <ul>
                <li>
                  <a href="https://www.instagram.com/shl_hk/"><img src="/images/pc/ig.png" /></a>
                </li>
                <li>
                  <a href="https://www.facebook.com/shlhk"><img src="/images/pc/fb.png" /></a>
                </li>
                <li>
                  <a href="https://www.youtube.com/watch?v=jBksP6soTUc"><img src="/images/pc/youtube.png" /></a>
                </li>
              </ul>
            </div>
            <div class="right">
              <ul>
                <li>
                  <img src="/images/mobile/stripe.png">
                </li>
                <li>
                  <img src="/images/mobile/wechatpay.png">
                </li>
                <li>
                  <img src="/images/mobile/alipay.png">
                </li>
              </ul>
            </div>
            <!-- <p>{{$t('home.Weaccept')}}<img src="/images/mobile/pcindex_15.png"></p> -->
          </div>

    </div>
    <div class="footerCpy">
            <p>Copyright {{currentYear}} © China Creation Limited <br> Powered by Eventizer
            <a href="https://eventizer.hk/" target="_blank">
              <img src="/images/mobile/footerlogo.png">
            </a>
            </p>
          </div>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';

@Component({
  components: {}
})
export default class InsFooter extends Vue {
  currentYear: number = 0;
  clickIndex: number = 0;
  footerMenus: any[] = [];
  showdownicon: boolean = false;
  // showMeun (item, index) {
  //   $('.sub' + index).slideToggle();
  // }
  showMeun (item, index) {
    this.clickIndex = index;
    item.showSub = !item.showSub;
    this.footerMenus.forEach((element, index) => {
      if (index !== this.clickIndex) {
        element.showSub = false;
      }
    });
  }
  closeSlideMenu (n) {
    this.$store.dispatch('isShowMenu', false);
  }
  toUrl (n) {
    if (!n.IsNewWin && n.Url) {
      window.location.href = n.Url;
    } else if (n.IsNewWin && n.Url) {
      window.open(n.Url);
    }
  }
  showdown () {
    this.showdownicon = !this.showdownicon;
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
      // this.footerMenus = result.ReturnValue.FooterMenus;
      let menus = JSON.parse(JSON.stringify(result.ReturnValue.FooterMenus));
        menus.forEach((element) => {
          element.showSub = false;
        });
        this.footerMenus = menus;
    });
  }
  created () {
    var date = new Date();
    this.currentYear = date.getFullYear();
    this.getMenu();
  }
  @Watch('$route', { deep: true })
  onIdChange () {
    $('.submeunMain').hide();
    let menus = JSON.parse(JSON.stringify(this.$store.state.footerMenus));
    menus.forEach((element) => {
      element.showSub = false;
    });
    this.footerMenus = menus;
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
.sub-comments-leave-active,
.sub-comments-enter-active {
  transition: max-height 0.3s linear;
}
.sub-comments-enter,
.sub-comments-leave-to {
  max-height: 0;
  // opacity: 0;
}
.sub-comments-enter-to,
.sub-comments-leave {
  max-height: 20rem;
}
// .submeunMain{
//   display: none;
// }
// .SubMeun{
//   display: none;
// }
// .SubMeun1{
//   display: none;
// }
#footer{
  width: 100%;

  // display: inline-block;
  .footer-box{
    width: 100%;
    margin: 0 auto;
    padding-top: 1rem;
    padding-bottom: 1rem;
    // background: url('/images/mobile/MobileIndex_04.jpg') no-repeat bottom center;
    background-size: cover;

    .footerAccept{
      width: 100%;
      padding: 0 1rem;
      box-sizing: border-box;
      >p{
        font-size: 1.4rem;
        color:#333;
        display: flex;
        align-items: center;
        justify-content: center;
        img{
          display: block;
          margin-left: 1rem;
        }
      }
      .left, .right{
        float: left;
        .title{
          font-size: 1.2rem;
          color: #999999;
          margin-bottom: 1rem;
        }
        ul{
          display: flex;
          li{
            margin-right: 0.5rem;
            &:last-child{
              margin-right: 0;
            }
            a{
              img{
                height: 2.5rem;
                display: block;
              }
            }
            img{
                height: 2.5rem;
                display: block;
              }
          }
        }
      }
      .right{
        float: right;
      }
    }
    .footerNav {
      width: 100%;
      margin: 0 auto;
      margin-top: 0;
      margin-bottom: 2rem;
      border-top: 1px solid #404040;
      > ul > li {
        width: 100%;
        display: inline-block;
        line-height: 3.5rem;
        text-align: left;
        margin-bottom: 0;
        position: relative;
        border-bottom: 1px solid #404040;
        overflow: hidden;
        padding: 0 1rem;
        box-sizing: border-box;
        > ul {
          position: relative;
          overflow: hidden;
        }
        > ul > li {
          width: 100%;
          display: inline-block;
          background: transparent;
          height: 3rem;
          line-height: 3rem;
          text-align: left;
          > a {
            font-size: 1.4rem;
            color: #999999;
            font-weight: 500;
            text-decoration: none;
            display: block;
            text-align: center;
            &:hover {
              text-decoration: underline;
              color: #fff;
            }
          }
        }
        > a {
          font-size: 1.3rem;
          color: #333;
          font-weight: 500;
          text-decoration: none;
          position: relative;
          display: block;
          text-align: center;
          // background: #ffffff;
          // margin-bottom: 0.5rem;
        }
        i.downIcon {
          background: url("/Images/mobile/arrow-down-back.png") no-repeat center center;
          background-size: contain;
          width: 1.2rem;
          height: 1.2rem;
          display: inline-block;
          position: absolute;
          right: 1rem;
          top: 1rem;
          transition: all 0.3s;
          transform: rotate(180deg);
        }
        i.upIcon {
          transform: rotate(360deg);
          transition: all 0.3s;
        }
      }
    }
    .contactBox{
    width: 100%;
    display: inline-block;
    margin-top: 2rem;
      .w50{
        width: 50%;
        float: left;
        >p:nth-child(1){
          font-size: 1.4rem;
          text-align: center;
          color: #FFF;
          padding-bottom: .5rem;
        }
        >p:nth-child(2){
            font-size:2.2rem;
            text-align: center;
            color: #FFF;
        }
      }
    }
    >p{
      width:90%;
      margin: 2rem auto;
      display: flex;
      align-items: center;
      justify-content: center;

      img{
        // width: 100%;
        height: 4rem;
        display: block;
        margin: 0 0.5rem;
      }
    }
  }
  .footerCpy{
      width: 100%;
      // display: inline-block;
      padding-top: 1rem;
      padding-bottom: 1rem;
      background-color: #fff;
      >p{
        color:#333;
        font-size: 1.2rem;
        text-align: center;
        img{
          width: 1.5rem;
          display: inline-block;
          margin-left: 1rem;
          vertical-align: middle;
        }
      }
    }
}
</style>
