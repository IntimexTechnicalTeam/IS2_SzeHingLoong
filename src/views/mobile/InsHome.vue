<template>
  <div class="home" v-cloak>
    <HomeBanner :initOptions="swiperOptionhome" :page="'Home'" :initSwiper="true" class="banner" />
    <HkAbout/>
    <HkPromotion />
    <PkNews/>
    <!-- <HkBranch /> -->
    <HkLiveBox />
    <PkMap/>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component({
  components: {
    HomeBanner: () => import('@/components/base/mobile/InsBanner.vue'),
    HkPromotion: () => import('@/components/hkTasteBusiness/mobile/home/HkPromotion.vue'),
    HkBranch: () => import('@/components/hkTasteBusiness/mobile/home/HkBranch.vue'),
    HkLiveBox: () => import('@/components/hkTasteBusiness/mobile/home/HkLiveBox.vue'),
    HkAbout: () => import('@/components/hkTasteBusiness/mobile/home/HkAbout.vue'),
    PkNews: () => import('@/components/hkTasteBusiness/mobile/home/PkNews.vue'),
    PkMap: () => import('@/components/hkTasteBusiness/mobile/home/PkMap.vue')
  }
})
export default class InsHome extends Vue {
  swiperOptionhome: object = {
    spaceBetween: 30,
    effect: 'fade',
    autoplay: {
      disableOnInteraction: false
    },
    pagination: {
      el: '.swiper-pagination',
      clickable: true
    }
  };
}
</script>

<style lang="less" scoped>
.banner {
  // min-height: 100vh;
  background-color: white;
}
</style>
