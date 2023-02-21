<template>
    <div class="memberlogin" v-click-outside="closeDialog">
        <div class="menberCentral" @click="menberCentral">
          <div class="menberCentral_logo">
            <img class="showMenberCentral" src="/images/mobile/Mobile-index_01.png">
            <p v-if="!$Storage.get('isLogin')">{{$t('Login.Loginregistration')}}</p>
            <p v-else>{{$t('Account.MemberCenter')}}</p>
          </div>
          <transition name="slide-fade">
            <div class="lang_flow" v-show="showMenberCentral" @click="memberCentral">
              <div data-to="/account/memberInfo" class="ii">{{$t('Account.MemberInformation')}}</div>
              <div data-to="/account/notification" class="ii">{{$t('Account.MyMessages')}}</div>
              <div data-to="/order/List" class="ii">{{$t('Account.MyOrder')}}</div>
              <div data-to="/account/deliveryAddress" class="ii">{{$t('Account.DeliveryAddress')}}</div>
              <div data-to="/account/mycoupon" class="ii">{{$t('MyCoupon.MyCoupon')}}</div>
              <div @click="logout()" class="ii">{{$t('Account.Logout')}}</div>
          </div>
          </transition>
        </div>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import Cookie from 'js-cookie';
@Component
export default class InsLangSwitch extends Vue {
  private showMenberCentral:boolean = false;

  closeDialog () {
    this.showMenberCentral = false;
  }
  menberCentral () {
    if (!this.$Storage.get('isLogin')) {
      window.location.href = '/account/login';
    } else {
      this.showMenberCentral = !this.showMenberCentral;
    }
  }
  memberCentral (e) {
    let target = e.target as HTMLElement;
    if (target.className === 'ii' && target.dataset.to) {
      this.$router.push({
        path: target.dataset.to
      });
      this.$store.dispatch('isShowMenu', false);
    }
  }
  logout () {
    this.$Api.member.logout().then((result) => {
      if (result) this.$message('Message.SucceedInOperating');
      this.$store.dispatch('Logout');
      this.$router.push('/');
      this.Reload();
    });
  }
}
</script>
<style scoped lang="less">
  .menberCentral{
    padding-left: 0.8rem;
    padding-right: 0.8rem;
    float: left;
    height: 2rem;
    // border-right: 1px solid #e6e6e6;
    display: flex;
    align-items: center;
    justify-content: center;
    .lang_flow{
      position: absolute;
      top: 4rem;
      left: 0;
      width: 100%;
      // background: #FFF;
      // background: url('/Images/mobile/Centerbg.png') no-repeat bottom center;
      background-color: #ab1638;
      background-size: cover;
      z-index: 999;
      padding: 1rem 0;
      // box-shadow: 0 3px 5px #d5d6d3;
      // box-shadow: 2px 5px 10px #d5d6d3;
      // border-radius: 8px;
      border-bottom-left-radius: 16px;
      border-bottom-right-radius: 16px;
      >div.ii{
        color:#333333;
        font-size: 1.2rem;
        height: 3.5rem;
        display: flex;
        justify-content: center;
        align-items: center;
        background-color: #fff;
        border-radius: 2px;
        box-shadow: 2px 2px 3px #777777;
        margin: 0 auto;
        margin-bottom: 1rem;
        width: 90%;
        img{
          width: 1.5rem;
          display: block;
          margin: unset;
          margin-right: 0.5rem;
        }
      }
      .title{
        // height: 4rem;
        padding: 1rem 0;
        text-align: center;
        span{
          font-size: 1.2rem;
          display: block;
          width: 100%;
          padding: 0.5rem 0;
          color: #fff;
        }
      }
    }
    img{
      width:1.5rem;
      margin: 0 auto;
      display: block;
    }
  }
  .menberCentral_logo{
    display: flex;
    justify-content: center;
    align-items: center;
    p{
      font-size: 1.2rem;
      color: #333333;
      margin-left: 0.5rem;
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
