<template>
  <div class="news fade-in-hot">
    <h2>{{ContentsName}}</h2>
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
  NewScroll () {
      let fadeInElements = document.getElementsByClassName('fade-in-hot');
      // console.log(document.getElementsByClassName('fade-in'), '滚动');
      for (var i = 0; i < fadeInElements.length; i++) {
        let elem = fadeInElements[i] as HTMLElement;
        if (this.isElemVisible(elem)) {
          elem.style.opacity = '1';
          elem.style.transform = 'translate(0, 0)';
          // fadeInElements.splice(i, 1); // 只让它运行一次
        }
      }
      // document.removeEventListener('scroll', this.handleScroll);
    }
    isElemVisible (el) {
      var rect = el.getBoundingClientRect();
      var elemTop = rect.top + 300; // 200 = buffer
      var elemBottom = rect.bottom;
      return elemTop < window.innerHeight && elemBottom >= 0;
    }
  mounted () {
    this.getNews();
    document.addEventListener('scroll', this.NewScroll);
  }
  destroyed() {
     document.removeEventListener('scroll', this.NewScroll);
  }
}
</script>

<style scoped lang="less">
.news {
  width: 94%;
  margin: 0 auto;
  padding-top: 5.5rem;
  padding-bottom: 3rem;
  overflow: hidden;
}
.swiper_news{
    margin-top: 2rem;
    position: relative;
    .swiperOptionNews{
      width: 80%;
      margin: 0 auto;
      /deep/ .swiper-slide{
        a{
          width: 100%;
          height: 15rem;
          box-shadow: 0 0 5px #efeded;
          border-radius: 5px;
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
        font-size: 1.2rem;
        color: #666666;
        margin-top: 2rem;
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
.news h2 {
  font-size: 2rem;
  color: #aa1638;
  text-align: center;
  margin-bottom: 2.5rem;
  // text-transform: uppercase;
}
.news li {
  margin-bottom: 2rem;
  min-height: 10rem;
  text-align: center;
  /deep/ p{
    font-size: 1.4rem;
    text-align: center;
    line-height: 2rem;
    color: #333;
    width: 100%;
  }
}
.news li a {
  width: 100%;
  height: 19.5rem;
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
  font-size: 1.5rem;
  color: #7e7e7e;
  text-align: left;
  height: 3rem;
  line-height: 3rem;
  margin-top: 1rem;
  border-bottom: 1px solid #bdbdbd;

}
.news li .news-title {
  width: 100%;
  font-size: 1.5rem;
  color: #424242;
  text-align: center;
  height: 4rem;
  line-height: 2rem;
  margin-top: 1rem;
  display:flex;
  line-clamp: 2;
  -webkit-box-orient: vertical;
  word-wrap: break-word;
}
.news .more {
  display: block;
  width: 94%;
  margin: 0 auto;
  overflow: hidden;
}
.news .more a {
  color: #7e7e7e;
  font-size: 1.5rem;
  float: right;
}
.fade-in-hot {
  opacity: 0;
  transition: 1s all ease-out;
  // transform: translate(0, -30px);
  box-sizing: border-box;
}

</style>
