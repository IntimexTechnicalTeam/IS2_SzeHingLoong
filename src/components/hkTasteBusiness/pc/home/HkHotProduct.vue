<template>
    <div class="indexSale fade-in">
        <div class="TitleBg"><div class="innerBox">{{$t('Cms.discounts')}}</div></div>
        <!-- <div class="indexHotSale">
          <div class="perSale" v-for="(val,index) in HotSales" :key="index">
                   <inProductWindow :item="val"  style="width:100%;"></inProductWindow>
            </div>
        </div> -->

        <div class="indexHotSale">
          <div class="HotSalebox">
            <swiper :options="swiperOptionHot" v-if="HotSales.length">
              <!-- slides -->
              <swiperSlide
                v-for="(val, index) in HotSales"
                :key="index"
              >
              <inProductWindow :item="val"></inProductWindow>
              </swiperSlide>

            </swiper>
            <div
                class="swiper-button-prev swiper-button-prev-hot"
                slot="button-prev"
              ></div>
              <div
                class="swiper-button-next swiper-button-next-hot"
                slot="button-next"
              ></div>
          </div>

          </div>
    </div>
</template>
<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
import inProductWindow from '@/components/hkTasteBusiness/pc/product/HkProductWindow.vue';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component({
  components: {
    inProductWindow,
    swiper,
    swiperSlide
  }
})
export default class PkHotProduct extends Vue {
    HotSales:any[]=[];

    swiperOptionHot: object = {
      slidesPerView: 4,
      spaceBetween: 28,
      // slidesPerGroup: 4,
      // loop: true,
      // loopFillGroupWithBlank: true,
      navigation: {
        nextEl: '.swiper-button-next-hot',
        prevEl: '.swiper-button-prev-hot'
      }
    }
    loadHotProducts () {
      var page = 'Home';
      this.$Api.promotion.getPromotion('Home', 4).then((result) => {
        if (result.Promotion.PrmtProductList.length > 0) {
          this.HotSales = result.Promotion.PrmtProductList.slice(0, 8);
        }
      });
    }
    get lang () {
      return this.$Storage.get('locale');
    }

    HOtScroll () {
    let fadeInElements = document.getElementsByClassName('fade-in');
    // console.log(document.getElementsByClassName('fade-in'), '滚动');
    for (var i = 0; i < fadeInElements.length; i++) {
      let elem = fadeInElements[i] as HTMLElement;
      if (this.isElemVisible(elem)) {
        elem.style.opacity = '1';
        // elem.style.transform = 'translate(0, 0)';
        // fadeInElements.splice(i, 1); // 只让它运行一次
      }
    }
    // document.removeEventListener('scroll', this.handleScroll);
  }
  isElemVisible (el) {
    var rect = el.getBoundingClientRect();
    var elemTop = rect.top + 100; // 200 = buffer
    var elemBottom = rect.bottom;
    return elemTop < window.innerHeight && elemBottom >= 0;
  }
    mounted () {
      this.loadHotProducts();
      document.addEventListener('scroll', this.HOtScroll);
    }
    destroyed () {
      document.addEventListener('scroll', this.HOtScroll);
    }
}
</script>
<style lang="less" scoped>
.TitleBg{
  width: 100%;
  // height: 79px;
  // background: url('/images/pc/titilebg.png') center no-repeat;
  background-size: contain;
  text-align: center;
  position: relative;
  margin-bottom: 36px;
  .innerBox{
    font-size: 30px;
    color: #aa1638;
    // text-transform: uppercase;
    font-weight: bold;
    // letter-spacing: 1px;
    // padding-top: 44px;
  }
}
.indexSale{
    width: 1200px;
    margin: 0 auto;
    // padding-top: 110px;
}
.indexSaleTitle{
    background: url('/images/pc/index_27.png') no-repeat center center;
    width: 544px;
    height: 114px;
    background-size: 100%;
    margin: 0 auto;
    margin-bottom: 30px;
}
.indexSaleTitleE{
    background: url('/images/pc/bigsales.png') no-repeat center center!important;
    width: 544px;
    height: 114px;
    background-size: 100%;
    margin: 0 auto;
    margin-bottom: 30px;
}
.indexHotSale{
    width: 1200px;
    margin: 0 auto;
    position: relative;
    .HotSalebox{
      width: 962px;
      margin: 0 auto;
      display: flex;
      flex-wrap: wrap;

    }
    /deep/ .swiper-container{
      width: 100%;
      // .swiper-wrapper{
      //   width: 962px;
      //   margin: 0 auto;
      // }

    }
    /deep/ .productMain{
      // border: 2px solid #ebd9b6;
    // border-radius: 6px;
    // overflow: hidden;
    padding-bottom: 35px;
    }
    .swiper-button-prev, .swiper-button-next{
        width: 20px;
        height: 37px;
      }
      .swiper-button-prev-hot{
        background: url(/images/pc/hotprev.png) no-repeat;
        left: 40px;
      }
      .swiper-button-next-hot{
        background: url(/images/pc/hotnext.png) no-repeat;
        right: 40px;
      }
}
.perSale{
    box-sizing: border-box;
    width: 22%;
    float: left;
    margin-right: 4%;
    padding: 1.5%;
}
.perSale:nth-child(4n){
     margin-right: 0%!important;
}
.fade-in {
      opacity: 0;
      transition: 0.5s all ease-out;
      // transform: translate(0, 30px);
      box-sizing: border-box;
      display: inline-block;
    }
</style>
