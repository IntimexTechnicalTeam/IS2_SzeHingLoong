<template>
    <div>
        <div class="header_logo" v-if="!this.FrontE.slideMenu.Embedded">
            <i class="el-icon-close" @click="closeSlideMenu"></i>
        </div>
        <div class="menu_top">
            <!-- <ins-fav  /> -->
             <ins-login class="fav" />
            <CodeSelect class="header-code" />
            <ins-lang-switch class="headerLang" />
        </div>
        <div id="menu">
            <Menu :backColor="'@base_color'" :textColor="'#fff'" :uniqueOpened="true" />
        </div>
        <!-- <div class="menu_footer">
            <div class="innerShare">
                <a href="https://www.facebook.com/hktastefood/" class="nav" target="_blank"><img src="/images/mobile/facebook.png"/></a>
                <a href="https://www.facebook.com/hktastefood/" class="nav" target="_blank"><img src="/images/mobile/ig.png"/></a>
                <a href="https://www.youtube.com/embed/videoseries?list=PLeU-XfKN4KcjVolI4daTvRI2oNOSLCILM"  class="nav" target="_blank"><img src="/images/mobile/youtube.png" /></a>
            </div>
        </div> -->
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';

@Component({
  components: {
    InsLogo: () => import('@/components/base/mobile/InsLogo.vue'),
    Menu: () => import('@/components/business/mobile/header/InsElMenu.vue'),
    InsLangSwitch: () => import('@/components/business/mobile/header/InsLangSwitch.vue'),
    InsFav: () => import('@/components/business/mobile/header/InsFav.vue'),
    CodeSelect: () => import('@/components/business/mobile/header/InsCodeSelect.vue'),
    InsLogin: () => import('@/components/business/mobile/header/InsLogin.vue')
  }
})
export default class InsMenuLayout extends Vue {
  showMemNav: boolean = false;
  searchKey: string = '';

  handleOpen (key, keyPath) {
    console.log(key, keyPath);
  }
  handleClose (key, keyPath) {
    console.log(key, keyPath);
  }

  closeSlideMenu () {
    this.$store.dispatch('isShowMenu', false);
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
      this.$store.dispatch('isShowMenu', !this.$store.state.isShowMenu);
    } else {
      this.$router.push({
        path: '/product/search/-'
      });
      this.$store.dispatch('isShowMenu', !this.$store.state.isShowMenu);
    }
  }

  get user () {
    return this.$store.state.user;
  }

  get isLogin () {
    return this.$store.state.isLogin;
  }
}
</script>

<style lang="less">
.sidebar-container {
    background-color: #fff !important;
}
.menu_footer span{
    display: flex;
    float: left;
    border:1px solid #eee;
    padding-top: .5rem;
    padding-bottom: .5rem;
    padding-left: 2rem;
    padding-right: 2rem;
    margin-right: 1rem;
    font-size: 1.4rem;
}
.innerShare{
    width: 90%;
    margin: 0 auto;
    display: flex;
    margin-top: 2rem;
    padding-bottom: 5rem;
    align-items: center;
    justify-content: center;
}
.innerShare a {
    display: inline-block;
    margin-right: 1rem;
    vertical-align: middle;
}
.innerShare a img {
    width: 3rem;
}
.searchBox{
    width: 90%;
    height: 4rem;
    margin: 0 auto;
    margin-top: 3rem;
    position: relative;
    overflow: hidden;
    margin-bottom: 2rem;
    border:1px solid #666666;
    input{
        width: calc(100% - 10px);
        height: 4rem;
        margin: 0 auto;
        text-indent: 10px;
        border:none;
    }
    .search_btn{
        position: absolute;
        right: 0px;
        top:0rem;
        width: 4rem;
        height: 4rem;
        background: #666666;
        display: flex;
        align-items: center;
        justify-content: center;
        img{
            width: 50%;
            margin: 0 auto;
            display: block;
        }
    }
}
#menu {
    .el-submenu__icon-arrow {
        display: none;
    }

    .el-submenu__title {
        padding-top: 0.375rem;
        padding-bottom: 0.375rem;
        // border: 1px solid #666;
        height: auto!important;
        line-height: unset;
        background-color:transparent!important;
        .name{
            font-size: 1.4rem!important;
            color:#fff;
            text-transform: uppercase;
            // letter-spacing: 2px;
        }
        i{
            color: #fff;
        }
    }

    .el-menu {
        width: 90%;
        margin: 0 auto;
        background-color: transparent;
        border: 0;
        margin-bottom: 1rem;
        margin-top: 1rem;
        .el-submenu__icon-arrow {
            display: block;
            font-size: 1.6rem;
            margin-top: -10px;
        }

        > li {
            height: auto;
            line-height: unset;
            text-align: center;
            margin-bottom: 1.5rem;
            // background: url('/images/mobile/meunbg.png') no-repeat top center;
            background-color: #8c122f !important;
            background-size: cover;
            border-radius: 3px;
            border: 1px solid #8c122f;
            box-sizing: border-box;
             >a {
                 color:#fff;
                //  background: #fff;
                 background-size: 100% 100%;
                 display:block;
                 width: 100%;
                 padding-top: 1rem;
                 padding-bottom: 1rem;
                 margin: 0 auto;
                //  border:1px solid #666666;
                 font-weight: 500;
                 text-transform: uppercase;
                 >b{
                     color:#FFF;
                     display: block;
                     width: 100%;
                     font-weight: 500;
                     text-transform: uppercase;
                    //  letter-spacing: 2px;
                     &:nth-child(1){
                        color:#fff;
                        font-weight: 500;
                        font-size: 1.4rem;
                     }
                     &:nth-child(2){
                         color:#fff;
                         font-size: 1.4rem;
                     }
                 }
            }

            a {
                text-decoration: none;
            }
            >ul{
                margin-top: 0;
                margin-bottom: 0;
                >li{
                    background: transparent;
                    border: none;
                    margin-bottom: 0;
                    >a{
                        color:#ffd9e2;
                        //  background: #fff;
                        background-size: 100% 100%;
                        display:block;
                        width: 100%;
                        padding-top: 0;
                        padding-bottom: 1rem;
                        margin: 0 auto;
                        //  border:1px solid #666666;
                        font-weight: 500;
                        text-transform: capitalize;
                        b{
                            text-transform: capitalize;
                            font-size: 1.2rem !important;
                            color: #ffd9e2 !important;
                            letter-spacing: 0;
                        }
                    }
                }
            }
        }

        li {
            line-height: unset;
            padding: 0 !important;
            min-width: unset;
        }
    }
}
#menu .is-opened > .el-submenu__title{
    background: transparent!important;
    color:#fff!important;
    .name{
        color:#FFF!important;
    }
}
#menu .is-opened > .el-submenu__title .el-submenu__icon-arrow{
    color:#fff!important;
}
</style>

<style scoped lang="less">
.header_logo {
    height: 7rem;
    position: relative;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 1rem 0 1.5rem;
    border-bottom: 4px solid #acbd30;
    background-color: #fff;

    .el-icon-close {
        color: #777777;
        font-size: 2.8rem;
    }

    a {
        width: 12rem;
    }

    .slide-menu {
        cursor: pointer;
    }
}

/deep/ .langSwitch {
    font-size: 1.5rem;
    color: #106919;
    text-align: center;
    p {
        font-size: 1.5rem;
        display: inline-block;
        color: #fff;
        margin: 0 0.8rem;

        &.active {
            color: #127437;
            font-weight: bold;
        }
    }
}
.menu_top{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 90%;
    margin: 1rem auto;
    /deep/ .fav{
        width: 33.33%;
        height: 2.5rem;
        border-right: 1px solid #e6e6e6;
        text-align: center;
        display: flex;
        justify-content: center;
    }
    /deep/ .main-code{
        width: 33.33%;
        height: 2.5rem;
        border-right: 1px solid #e6e6e6;
        text-align: center;
        display: flex;
    }
    /deep/ .langSwitch{
        width: 33.33%;
        height: 2.5rem;
        text-align: center;
        display: flex;
    }
}
</style>
