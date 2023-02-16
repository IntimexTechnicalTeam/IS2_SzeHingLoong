<template>
  <a href="/" class="logo">
    <img class="logo1" src="/images/mobile/logo2_JPG.png"> <img class="logo2" src="/images/mobile/pcindex_09.png">
  </a>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component
export default class InsLogo extends Vue {

}
</script>
<style scoped lang="less">
.logo {
  display: flex;
    align-items: center;
  img {
    height: 3rem;
    display: block;
    object-fit: contain;
  }
  .logo1{
    margin-right: 10px;
  }
}
</style>
