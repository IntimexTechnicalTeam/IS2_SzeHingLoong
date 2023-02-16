<template>
<div class="header-layout"  v-cloak>
  <div class="headerBg">
      <div class="headerTop fix">
        <div class="header_logo">
          <!-- logo开始 -->
          <div class="logoBox">
              <a href="/"><img class="logo1" src="/images/pc/logo2_JPG.png"> <img class="logo2" src="/images/pc/pcindex_09.png"></a>
          </div>
          <!-- logo结束 -->
        </div>
        <div class="header_menu" >
          <!-- 导航栏开始 -->
          <Menu />
          <!-- 导航栏结束 -->
        </div>
        <div class="inner" :class="{'menu_Eng': $Storage.get('locale') === 'E'}">

            <!-- 我的喜爱开始 -->
            <!-- <div class="cartTop">
                <router-link to="/account/MyFavorite">
                  <i class="handle-icon fav-icon"></i>
                </router-link>
            </div> -->
            <!-- 我的喜爱结束 -->
            <!-- 会员登陆开始 -->
            <InsLogin class="memberLogin"></InsLogin>
            <!-- 搜索框开始 -->
            <div class="search" v-click-outside="closeDialog">
                <a href="javascript:;" @click="isShowSearch = !isShowSearch"><img  src="/images/pc/searchbtn.png"></a>
                <transition name="slide-fade">
                    <div class="search-box" v-if="isShowSearch">
                      <div>
                        <span class="searchBtn" @click="searchFun(key)"></span>
                        <input type="text" v-model="key" @keyup.enter="searchFun(key)" />
                      </div>
                    </div>
                </transition>
            </div>
            <!--搜索框结束  -->
            <!-- 购物车开始 -->
            <Shopcart class="memberLogin"></Shopcart>
            <!-- 购物车结束 -->
            <!-- 切换语言开始 -->
            <CodeSelect/>
            <div class="langBox">
                <InsLangSwitch></InsLangSwitch>
            </div>
            <!-- 切换语言结束 -->
        </div>
      </div>
  </div>
</div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component({
  components: {
    Menu: () =>
      import('@/components/business/pc/header/InsMenu.vue'),
    Shopcart: () =>
      import('@/components/business/pc/header/InsShoppingCart.vue'),
    InsLogin: () =>
      import('@/components/business/pc/header/InsLogin.vue'),
    InsLangSwitch: () =>
      import('@/components/business/pc/header/InsLangSwitch.vue'),
    CodeSelect: () =>
      import('@/components/business/pc/header/InsCodeSelect.vue')
  }
})
export default class DefaultHeader extends Vue {
  @Prop() private showInFixed!: boolean;

  private key: string = '';
  isShowSearch: boolean = false;

  closeDialog () {
    this.isShowSearch = false;
  }

  getMenu () {
    this.$Api.promotion
      .getMenu()
      .then(result => {
        this.$store.dispatch('setHeaderMenus', result.ReturnValue.HeaderMenus);
        this.$store.dispatch('setFooterMenus', result.ReturnValue.FooterMenus);
      })
      .catch(error => {
        console.log(error);
      });
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
    } else {
      this.$router.push({
        path: '/product/search/-'
      });
    }
    this.isShowSearch = false;
  }
  get currentlang () {
    return this.$i18n.locale;
  }
  private changLange (lang) {
    this.$Api.member
      .setUILanguage(lang)
      .then(result => {
        this.$i18n.locale = lang;
        localStorage.setItem('locale', lang);
        this.Reload();
      })
      .catch(error => {
        console.log(error);
      });
  }

  created () {
    this.$store.dispatch('setShopCart', this.$Api.shoppingCart.getShoppingCart());
  }

  mounted () {
    this.getMenu();
  }
}
</script>

<style scoped lang="less">
.showMenuYes{
  height:151px;
  transition:all 1s;
}
#header{
  z-index: 9999;
  top:0px;
  width: 100%;
  display: flex;
}
.headerBg{
   width: 100%;
   background:#fff;
   background-size: cover;
   display: inline-block;
   box-shadow: 0 0 10px 0 #d4d5d1;
}
.headerTop{
    width: 1200px;
    margin: 0 auto;
    padding-top: 10px;
    padding-bottom: 10px;
    // height: 34px;
    .header_logo{
      float: left;
    }
}

.headerTop .inner{
        float: right;
        display: flex;
    align-items: center;
    height: 68px;
    }
.headerTop .menu_Eng{
  /deep/ .handle-icon span{
    display: flex;
    align-items: center;
  }
}
.search {
  position: relative;
  margin: 0 15px;
          // padding-right: 1rem;
          // padding-left: 1rem;
          img {
            // width: 1.3rem;
            height: 20px;
            display: block;
          }
          .search-box {
      position: absolute;
      width: 340px;
      z-index: 2;
      // padding: 0 1.5rem;
      box-sizing: border-box;
      display: flex;
      right: -60px;
      top: 56px;

      >div{
        width: 100%;
        position: relative;

        .searchBtn{
          width: 22px;
          height: 22px;
          display: inline-block;
          background: url(/images/pc/searchbtn.png) no-repeat center center;
          background-size: cover;
          -ms-flex-negative: 0;
          flex-shrink: 0;
          cursor: pointer;
          position: absolute;
          top: 12px;
          right: 12px;
          &::before{
            content: '';
            width: 1px;
            height: 30px;
            background-color: #e6e6e6;
            position: absolute;
            left: -10px;
            top: -2px;
          }
        }

        input {
          width: 100%;
          height: 45px;
          font-size: 14px;
          color: #999999;
          appearance: none;
          -webkit-appearance: none;
          -moz-appearance: none;
          -ms-appearance:none;
          outline: none;
          border: 0;
          background-repeat: no-repeat;
          background-position-y: 4px;
          padding: 0 45px 0 10px;
          box-sizing: border-box;
          background-color: transparent;
          border-radius: 5px;
          background-color: #fff;
          box-shadow: 0px 2px 3px #e5ddd2;
        }
      }
    }
        }
// .search-box {
//     border: 1px solid #808080;
//     width: 340px;
//     display: flex;
//     float: left;
//     align-items: center;
//     margin-right: 20px;
// }
// .searchBtn{
//     width: 25px;
//     height: 25px;
//     display: inline-block;
//     background: url('/images/pc/pcindex_03.png') no-repeat center center;
//     background-size: 100%;
//     cursor: pointer;
// }
// .search-box .inputBox {
//     width: 305px;
//     float: left;
//     border:none;
//     background: transparent;
//     line-height: 30px;
//     text-indent: 10px;
// }
// .search-box  .btn-send {
//     position: absolute;
//     right: 10px;
//     top: 6px;
//     line-height: 1;
//     background: none;
//     border: none;
//     color: #555;
// }
// .search-box form {
//     position: relative;
//     font-size: 12px;
// }
// .search-box input {
//     font-family: inherit;
//     font-size: inherit;
//     font-style: inherit;
//     font-weight: inherit;
//     outline: 0;
// }
.memberLogin{
    display: flex;
    float: left;
    align-items: center;
    position: relative;
    // margin-right: 20px;
}
.cartTop{
    display: flex;
    float: left;
    align-items: center;
    position: relative;
    margin-left: 20px;
}
.langBox{
    display: flex;
    float: left;
    align-items: center;
    position: relative;
    justify-content: center;
    margin-left: 20px;
}
.langBox a{
   color:#323232;
   font-size: 14px;
    line-height: 32px;
    margin-right: 10px;
    width: 40px;
    display: inline-block;
    text-align: center;
}
.langBox a:nth-child(2){
    margin-right: 0px!important;
}
.langActive{
    background: #808080;
    color:#FFF!important;
}
.logoBox{
    width: 100%;
    // text-align: center;
    // display: flex;
    // align-items: center;
    // justify-content: center;
    // padding-top: 40px;
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

.fav-icon {
    background: url('/images/pc/pcindex_08.png') no-repeat center center;
    display: inline-block;
    width: 25px;
    height: 25px;
    background-size: contain;
}

// new css
.header-layout {
 /deep/ .header_menu {
  //  width: 1200px;
  //  margin: 30px auto 10px;
   float: left;

  > ul {
     > li {
      float: left;
      display: flex;
      align-items: center;
      position: relative;
      width: auto;
      padding: 0 26px;
      > a {
        // width: 100%;
        font-size: 18px;
        color: #333333;
        display: block;
        text-align: center;
        font-weight: 500;
        text-transform: capitalize;
        padding: 0;
        width: auto;
        line-height: 68px;
      }

      &:hover{
        > a  {
          // background: @base_color;
          color: @base_color;
        }
      }

      >ul {
          box-shadow: 0 2px 5px #ccc;
          width: 250px;
          position: absolute;
          left: 50%;
          transform: translateX(-50%);
          top: 100%;
          // padding-top: 10px;
        >li {
          border: 0;
          > a {
            font-size: 16px;
            color: #999999;
            display: block;
            text-align: center;
            font-weight: 500;
            text-transform: capitalize;
            // padding: 10px 5px;
            // font-family: 'Poppins-Light';
          }

          &:hover{
             > a {
              background: @base_color;
              // background: url('/images/pc/menubg.png');
              // background-size: cover;
              color: #fff;
            }
          }
        }
      }
     }
   }
 }
 /deep/ .menu_Eng{
  > ul > li {
        padding: 0 22px;
    > a{
      font-size: 16px;
    }
  }
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
