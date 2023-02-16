<template>
  <div class="productSearchSwiper">
    <p class="gradient"></p>
    <div class="swiper-container swiper-container-hot">
        <swiper :options="swiperOption" ref="mySwiper">
        <!-- slides -->
        <swiperSlide v-for="(slide, index) in hotProducts" :key="index">
          <img :src="slide.Image" class="BannerImg">
        </swiperSlide>
        </swiper>
    </div>
    <div class="TitleBg"><div class="innerBoxText">{{TitleName}}</div></div>
  </div>
</template>
<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component({ components: { swiper, swiperSlide } })
export default class PkProductListSwiper extends Vue {
    hotProducts:any[]=[];
    bannerImg: string = '';
    // @Prop() private TitleName!: string;
    TitleName:string = '';
    swiperOption: object = {
      pagination: {
        el: '.swiper-pagination',
        clickable: true
      }
    };
    loadHotProducts () {
      this.$Api.promotion.getPromotion('PromProductList', 0).then((result) => {
        this.TitleName = result.Promotion.Name;
        if (result.Promotion.BannerList.length > 0) {
          this.hotProducts = result.Promotion.BannerList;
        }
      });
    }
    mounted () {
      this.loadHotProducts();
    }
}
</script>
<style lang="less">
.productSearchSwiper  .swiper-pagination-bullet {
    width: 1rem!important;
    height: 1rem!important;
    margin-left: 5px;
}
.productSearchSwiper  .swiper-pagination-bullet-active{
    background: #f7a13a !important;
}
.productSearchSwiper   .swiper-pagination{
    margin-top: -4rem;
    transform: translateX(-50%) translateY(-50%);
    left: 50%;
}
</style>
<style lang="less" scoped>
.productSearchSwiper{
  position: relative;
}
.productSearchSwiper{
  min-height: 10rem;
}
.BannerImg{
    width: 100%;
    display: block;
}
.TitleBg{
  // width: 500px;
  // height: 70px;
  // border:1px solid #ffffff;
  margin: 0 auto;
  padding: 10px;
  // margin-bottom: 20px;
  position: absolute;
  top: 50%;
  left:50%;
  z-index: 999;
  transform: translateX(-50%) translateY(-50%);
  .innerBoxText{
    font-size: 2.4rem;
    color: #aa1638;
    // text-transform: uppercase;
    // letter-spacing: 1px;
    // padding-top: 44px;
    font-weight: bold;
  }
}
</style>
