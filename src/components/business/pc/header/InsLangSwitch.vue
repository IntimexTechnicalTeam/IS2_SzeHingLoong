<template>
    <div class="langSwitch">
        <!-- <p @click="changeLang('S')" :class="{'active': $Storage.get('locale') === 'S'}">简</p> |
        <p @click="changeLang('C')" :class="{'active': $Storage.get('locale') === 'C'}">繁</p> |
        <p @click="changeLang('E')" :class="{'active': $Storage.get('locale') === 'E'}">Eng</p> -->

      <!-- <p @click="changeLang(lang.value)" v-for="(lang,index) in FrontE.langList" :key="index" :class="{'active': $Storage.get('locale') === lang.value}">{{lang.name}}</p> -->
      <select  v-model="currentlang">
        <option :value="lang.value" v-for="(lang,index) in FrontE.langList" :key="index">{{lang.name}}</option>
      </select>
    </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import Cookie from 'js-cookie';
@Component
export default class InsLangSwitch extends Vue {
  get currentlang () {
    return this.$i18n.locale;
  }
  set currentlang (val) {
    this.changeLang(val);
  }
  changeLang (lang) {
    this.$Api.member.setUILanguage(lang).then((result) => {
      this.$i18n.locale = lang;
      localStorage.setItem('locale', lang);
      this.Reload();
    }).catch((error) => {
      console.log(error);
    });
 }
}
</script>
<style scoped lang="less">
.langSwitch {
  width: 54px;
    p {
      display: inline-block;
      padding: 5px 10px;
      cursor: pointer;
    }
    .active{
      background: grey;
      color: #fff!important;
    }
    select{
      width: 100%;
      background: transparent url('/images/pc/arrow-down-back.png')  95% 50% no-repeat;
      background-size: 11px;
      border:none;
      box-sizing: border-box;
      text-align: left;
      color:#666666;
      appearance: none;
      -moz-appearance: none;
      -webkit-appearance: none;
      // padding-left: .8rem;
      font-size: 16px;
      outline: none;
      text-transform: uppercase;
      option{
        color:#666666;
        outline: none;
      }
    }
}
</style>
