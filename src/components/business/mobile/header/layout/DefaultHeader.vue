<template>
    <div class="header-box">
      <!-- <div class="headerTop">
          <ins-login />
          <ins-fav  />
          <shopcart class="shoppingcart" />
          <CodeSelect class="header-code" />
          <ins-lang-switch class="headerLang" />
      </div> -->
        <div class="flex-box fix">
          <div class="left">
            <ins-logo />
          </div>
          <div class="right">
            <!-- <ins-login /> -->
            <shopcart class="shoppingcart" />
            <!-- 搜索框 -->
            <div class="search" v-click-outside="closeDialog">
                <a href="javascript:;" @click="isShowSearch = !isShowSearch"><img  src="/images/mobile/searchbtn.png"></a>
                <transition name="slide-fade">
                    <div class="search-box" v-if="isShowSearch">
                      <div>
                        <span class="searchBtn" @click="searchFun(key)"></span>
                        <input type="text" v-model="key" @keyup.enter="searchFun(key)" />
                      </div>
                    </div>
                </transition>
            </div>
            <!-- <ins-menu /> -->
            <!-- <ins-menu :layout="1" /> -->
            <img class="slide-menu" src="/images/mobile/Mobile-index_05.png" @click="showSlideMenu" v-show="!this.$store.state.isShowMenu" />
            <img class="close-meun" src="/images/mobile/out.png" @click="showSlideMenu" v-show="this.$store.state.isShowMenu"  />
          </div>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';

@Component({
  components: {
    InsLogo: () => import('@/components/base/mobile/InsLogo.vue'),
    InsLangSwitch: () => import('@/components/business/mobile/header/InsLangSwitch.vue'),
    InsLogin: () => import('@/components/business/mobile/header/InsLogin.vue'),
    InsFav: () => import('@/components/business/mobile/header/InsFav.vue'),
    shopcart: () => import('@/components/business/mobile/header/InsShoppingCart.vue'),
    InsMenu: () => import('@/components/business/mobile/header/InsMenu.vue'),
    CodeSelect: () =>
      import('@/components/business/mobile/header/InsCodeSelect.vue')
  }
})
export default class DefaultHeader extends Vue {
  @Prop() private showInFixed!: boolean;

  private key: string = '';
  isShowSearch: boolean = false;

  closeDialog () {
    this.isShowSearch = false;
  }

  searchFun (key) {
    this.$store.dispatch('setSearchKey', key);
    if (key !== '') {
      this.$router.push({
        path: '/product/search',
        name: 'productSearch',
        params: {
          key: key
        }
      });
      this.$store.dispatch('isShowMenu', this.$store.state.isShow);
    } else {
      this.$router.push({
        path: '/product/search/-'
      });
      this.$store.dispatch('isShowMenu', this.$store.state.isShow);
    }
    // this.showSlideMenu();
    this.key = '';
    this.isShowSearch = false;
  }

  showSlideMenu () {
    let isShow = !JSON.parse(JSON.stringify(this.menuShow));
    this.$store.dispatch('isShowMenu', isShow);
  }

  get menuShow () {
    return this.$store.state.isShowMenu;
  }

  get isMobile () {
    return this.$store.state.isMobile;
  }
  get ShopCart () {
    return this.$store.state.shopCart;
  }
  created() {
    this.$store.dispatch('setShopCart', this.$Api.shoppingCart.getShoppingCart());
  }
}
</script>

<style scoped lang="less">
    .header-box {
      .flex-box {
        border-bottom: 6px solid #ab1638;
        height: 5rem;
        background-color: @positive_color;
        position: relative;
        // display: flex;
        // justify-content: space-between;
        // align-items: center;
        padding: 0 1.5rem;
        // box-shadow: 0 0 5px #dfd9ce;
        z-index: 10000;

        .logo {
          // width: 10rem;
          margin: 0 auto;
        }

        .slide-menu {
          cursor: pointer;
          width: 2rem;
          display: block;
        }
        .close-meun {
          cursor: pointer;
          width: 2rem;
          display: block;
        }
        .left{
          float: left;
          width: 16rem;
          height: 100%;
          display: flex;
          justify-content: center;
          align-items: center;
        }
        .right{
          float: right;
          display: flex;
          justify-content: right;
          align-items: center;
          height: 100%;
          .search {
          padding-right: 1rem;
          padding-left: 1rem;
          img {
            width: 2rem;
            display: block;
          }
          .search-box {
      position: absolute;
      width: 100%;
      z-index: 2;
      padding: 0 1.5rem;
      box-sizing: border-box;
      display: flex;
      left: 0;
      top: 6.5rem;

      >div{
        width: 100%;
        position: relative;

        .searchBtn{
          width: 3rem;
          height: 2.5rem;
          display: inline-block;
          background: url(/images/mobile/searchbtn.png) no-repeat center center;
          background-size: 60%;
          -ms-flex-negative: 0;
          flex-shrink: 0;
          cursor: pointer;
          position: absolute;
          top: 0.5rem;
          right: 0.5rem;
          border-left: 1px solid #e6e6e6;
        }

        input {
          width: 100%;
          height: 3.5rem;
          font-size: 1.2rem;
          color: #999999;
          appearance: none;
          -webkit-appearance: none;
          -moz-appearance: none;
          -ms-appearance:none;
          outline: none;
          border: 0;
          background-repeat: no-repeat;
          background-position-y: 4px;
          padding: 0 4rem 0 1.5rem;
          box-sizing: border-box;
          background-color: transparent;
          border-radius: 5px;
          background-color: #fff;
          box-shadow: 0px 2px 3px #e5ddd2;
        }
      }
    }
        }
        }
      }
    }
.headerTop{
  width: 100%;
  display: flex;
  background: #fff;
  position: relative;
  border-bottom: 1px solid #e6e6e6;
  flex-wrap: wrap;
  .shoppingcart{
      padding-left: 1rem;
      padding-right: 1rem;
      float: left;
      height: 3.5rem;
      border-right: 1px solid #e6e6e6;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  .headerLang{
    width:20%;
    border-left: 1px solid #e6e6e6;
    position: absolute;
    right: 0px;
    height: 3.5rem;
    padding-left: 1rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .header-code{
    width:20%;
    border-left: 1px solid #e6e6e6;
    position: absolute;
    right: 25%;
    height: 3.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }
}
/* 可以设置不同的进入和离开动画 */
/* 设置持续时间和动画函数 */
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .3s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active for below version 2.1.8 */ {
  transform: translateY(-10px);
  opacity: 0;
}
</style>
