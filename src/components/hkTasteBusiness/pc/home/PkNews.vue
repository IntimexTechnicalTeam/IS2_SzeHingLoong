<template>
  <div class="news fade-in">
    <div class="TitleBg"><div class="innerBox">{{ContentsName}}</div></div>
    <div class="swiper_news">
      <swiper class="swiperOptionNews" :options="swiperOptionNews" v-if="lastestContents.length>0">
      <!-- slides -->
      <swiperSlide class="fade-in" v-for="(slide, index) in lastestContents" :key="index">
          <router-link :to="'/cms/content/'+slide.Id">
            <img :src="slide.Cover" class="NewsPart" />
          </router-link>
          <p class="news-title">{{slide.Title}}</p>
      </swiperSlide>
      </swiper>
      <!-- Optional controls -->
      <!-- <div class="swiper-pagination swiper-pagination-banner" slot="pagination"></div> -->
      <div
        class="swiper-button-prev swiper-button-prev-News"
        slot="button-prev"
      ></div>
      <div
        class="swiper-button-next swiper-button-next-News"
        slot="button-next"
      ></div>
    </div>
    <!-- <ul>
      <li v-for="(n,index) in lastestContents" :key="index">
        <router-link :to="'/cms/content/'+n.Id">
          <img :src="n.Cover" class="NewsPart" />
        </router-link>
        <p class="news-date">{{n.CreateDate}}</p>
        <p class="news-title">{{n.Title}}</p>
      </li>
    </ul> -->
    <!-- <p class="more">
      <a href="#">{{$t('home.More')}}></a>
    </p> -->
  </div>
</template>
<script lang="ts" scoped>
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component({ components: { swiper, swiperSlide } })
export default class PkNews extends Vue {
  lastestContents: any[] = [];
  ContentsName: string = '';
  swiperOptionNews: object = {
    slidesPerView: 2,
    spaceBetween: 20,
    navigation: {
      nextEl: '.swiper-button-next.swiper-button-next-News',
      prevEl: '.swiper-button-prev.swiper-button-prev-News'
    },
    observer: true,
    observeParents: true
  };
  getNews () {
    var cond = {
      Page: 1,
      PageSize: 2,
      catId: 40113
    };
    this.$Api.cms.getLastestContents(cond).then(result => {
      result.Data.forEach(function (item) {
        item.CreateDate = item.CreateDate.substring(
          0,
          item.CreateDate.indexOf(' ')
        );
      });
      this.lastestContents = result.Data;
      this.ContentsName = result.Data[0].Category.Name;
    });
  }
  NEWScroll () {
    let fadeInElements = document.getElementsByClassName('fade-in');
    for (var i = 0; i < fadeInElements.length; i++) {
      let elem = fadeInElements[i] as HTMLElement;
      if (this.isElemVisible(elem)) {
        elem.style.opacity = '1';
      }
    }
  }
  isElemVisible (el) {
    var rect = el.getBoundingClientRect();
    var elemTop = rect.top + 250; // 200 = buffer
    var elemBottom = rect.bottom;
    return elemTop < window.innerHeight && elemBottom >= 0;
  }
  mounted () {
    this.getNews();
  document.addEventListener('scroll', this.NEWScroll);
  }
  destroyed () {
    document.removeEventListener('scroll', this.NEWScroll);
  }
}
</script>

<style scoped lang="less">
.news {
  width: 100%;
  /*height:800px;*/
  padding-bottom: 40px;
  padding-top: 40px;
  box-sizing: border-box;
  .TitleBg{
  width: 100%;
  // height: 79px;
  // background: url('/images/pc/titilebg.png') center no-repeat;
  background-size: contain;
  text-align: center;
  position: relative;
  .innerBox{
    font-size: 30px;
    color: #aa1638;
    // text-transform: uppercase;
    font-weight: bold;
    // letter-spacing: 1px;
    // padding-top: 44px;
  }
}
  .swiper_news{
    width: 1200px;
    margin: 0 auto;
    margin-top: 40px;
    position: relative;
    .swiperOptionNews{
      width: 1080px;
      padding: 6px;
      margin: 0 auto;
      /deep/ .swiper-slide{
        a{
          width: 100%;
          height: 253px;
          box-shadow: 0 0 5px #efeded;
          border-radius: 3px;
          overflow: hidden;
          display: block;
        }
        img{
          width: 100%;
          height: 100%;
          display: block;
          object-fit: cover;
          object-position: 50% 50%;
          border: 6px solid #fff;
          box-sizing: border-box;
        }
      }
      .news-title{
        text-align: center;
        font-size: 18px;
        color: #666666;
        margin-top: 20px;
      }
    }
    .swiper-button-prev, .swiper-button-next{
      margin-top: -3rem;
    }
    .swiper-button-next-News{
        width: 16px;
        height: 28px;
        background: url('/images/mobile/right.png') no-repeat right;
      }
      .swiper-button-prev-News{
        width: 16px;
        height: 28px;
        background: url('/images/mobile/left.png') no-repeat left;
      }
  }
  .fade-in {
      opacity: 0;
      transition: 0.5s all ease-out;
      // transform: translate(0, 30px);
      box-sizing: border-box;
      display: inline-block;
    }
}
.news h2 {
  font-size: 28px;
  color: #383838;
  text-align: center;
  margin-bottom: 65px;
}
.news ul {
  width: 1120px;
  margin: 0 auto;
  overflow: hidden;
}
.news li {
  width: 326px;
  margin-right: 71px;
  float: left;
  box-sizing: border-box;
}
.news li:nth-child(3n) {
  margin-right: 0;
}
.news li a {
  display: block;
  width: 100%;
  height: 210px;
  background-color: #ededed;
  display: flex;
  justify-content: center;
  align-items: center;
}
.news li a img {
  max-width: 100%;
  max-height: 100%;
}
.news li .news-date {
  display: inline-block;
  width: 100%;
  font-size: 16px;
  color: #7e7e7e;
  text-align: left;
  height: 48px;
  line-height: 60px;
  border-bottom: 1px solid #bdbdbd;
}
.news li .news-title {
  width: 100%;
  font-size: 16px;
  color: #424242;
  text-align: left;
  height: 40px;
  line-height: 20px;
  margin-top: 10px;
  display: flex;
  display: -webkit-box;
  display: -moz-box;
  line-clamp: 2;
  -moz-line-clamp: 2;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  word-wrap: break-word;
}
.news .more {
  display: block;
  width: 1120px;
  margin: 0 auto;
  overflow: hidden;
  margin-top: 75px;
}
.news .more a {
  color: #7e7e7e;
  font-size: 16px;
  float: right;
}
.fade-in {
      opacity: 0;
      transition: 0.5s all ease-out;
      // transform: translate(0, 30px);
      box-sizing: border-box;
      display: inline-block;
    }
</style>
