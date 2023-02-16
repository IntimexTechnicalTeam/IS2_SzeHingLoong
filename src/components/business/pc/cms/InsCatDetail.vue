<template>
  <div id="container" class="catDetail" :class="{'catDetailback' : NewcateId == '50113'}">
    <div class="DetailTitle" v-if="NewcateId != '50113'">
      <img :src="cmsCategory.ImagePath" v-if="cmsCategory.ImagePath">
      <div class="TitleBg" v-else>
        <div class="innerBoxText">{{cmsCategory.Name}}</div>
      </div>
    </div>

    <div class="catContent">
        <template v-if="cmsCategory.PageStyle === '0' || cmsCategory.PageStyle === '1'">
          <div v-if="NewcateId == '50113'">
            <div class="catCotent_text">
              <div class="catCotent_title">{{cmsCategory.Name}}</div>
              <div v-html="cmsCategory.Content" class="layer"></div>
            </div>
            <div class="background" :style="'background-image:url(' + cmsCategory.ImagePath + ')'"></div>
          </div>
          <div v-else>
            <div v-html="cmsCategory.Content" class="layer"></div>
          </div>
        </template>

        <ins-cat-layout2 :catData="cmsCatTree" :cmsData="contentList" @changeCatSelect="changeCatSelect" v-if="cmsCategory.PageStyle === '2'" />

        <ins-cat-layout3 :cmsData="contentList" v-if="cmsCategory.PageStyle === '3'" />

        <ins-cat-layout4 :cmsData="contentList" :pager="pager" v-if="cmsCategory.PageStyle === '4'" />
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
@Component({
  components: {
    // InsBanner: () => import('@/components/base/InsBanner.vue'),
    InsCatLayout2: () => import('@/components/business/pc/cms/InsCatLayout2.vue'),
    InsCatLayout3: () => import('@/components/business/pc/cms/InsCatLayout3.vue'),
    InsCatLayout4: () => import('@/components/business/pc/cms/InsCatLayout4.vue')
  }
})
export default class insNews extends Vue {
    cmsCategory: object = {}; // cms下单个目录的信息
    cmsCatTree: object[] = []; // cms目录
    contentList: object[] = []; // cms内容列表
    catId: number = 0; // Tree点击获取内容列表的目录id
    PageStyle: string = '0'; // catDetail页面类型
    pager: any = {
      currentPage: 1, // 当前页
      pageSize: 12, // 每页显示条目个数
      totalRecord: 0 // 总条目数
    }
    NewcateId: number = 0;

    // 根据设备类型获取CMSCategory信息
    getCategoryByDevice () {
      this.$Api.cms.getCategoryByDevice({ CatId: this.id, IsMobile: false }).then((result) => {
        this.cmsCategory = result;
        this.PageStyle = result.PageStyle;
        this.NewcateId = result.Id;
        switch (result.PageStyle) {
            case '2':
              this.getCategoryTree();
              break;
            case '3':
              this.getSubCatContents();
              break;
            case '4':
              this.getContentsByCatId();
              break;
        }

        this.$nextTick(() => {
          if (result.Name) document.title = result.Name;
          (document.getElementsByName('keywords')[0] as any).content = result.SeoKeyword;
          (document.getElementsByName('description')[0] as any).content = result.SeoDesc;
          (document.getElementsByName('twitter:description')[0] as any).content = result.SeoDesc;
          (document.getElementsByName('twitter:title')[0] as any).content = result.Name;
        });
      }).catch((error) => {
        console.log(error, 'error');
        this.$message({
          message: error,
          type: 'error'
        });
      });
    }

    // 获取cms该id下所有的Category
    getCategoryTree () {
      this.$Api.cms.getCategoryTree({ id: this.id }).then((result) => {
        if (result && result.length) {
          this.cmsCatTree = result;
          this.catId = result[0].Id;
          this.getContentsByCatId();
        } else {
          this.getContentsByCatId();
        }
      });
    }

    // 获取cms该id下所有的cms
    getContentsByCatId () {
      let catId = this.catId || this.id;
      this.$Api.cms.getContentsByCatId(catId, this.pager.currentPage, this.pager.pageSize).then((result) => {
        this.contentList = result.Data;

        result.Data.forEach(function (i) {
          var newDate = new Date(i.CreateDate.replace(/-/g, '/'));
          i.CreateDate = newDate.getFullYear() + '-' + (newDate.getMonth() + 1) + '-' + newDate.getDate();
        });
        this.pager.totalRecord = result.TotalRecord;
      });
    }

    // 获取 Category 和所有 SubCategory 的 cms 列表
    getSubCatContents () {
      this.$Api.cms.getSubCatContents({ CatId: this.id, IsMobile: false }).then((result) => {
        console.log(result, ' 获取 Category 和所有 SubCategory 的 cms 列表');
        this.contentList = result.Data;
      });
    }

    // 树形控件选择的cms目录改变
    changeCatSelect (Id) {
      this.catId = Id;
      this.getContentsByCatId();
    }

    get id () {
      return this.$route.params.id;
    }

    mounted () {
      this.getCategoryByDevice();
    }

    @Watch('id', { deep: true })
    onKeyChange () {
      this.cmsCategory = {};
      this.cmsCatTree = [];
      this.contentList = [];
      this.catId = 0;
      this.getCategoryByDevice();
    }

    @Watch('pager.currentPage', { deep: true })
    onPagerChange() {
      this.getContentsByCatId();
    }
}
</script>
<style scoped lang="less">
.catDetail {
  .DetailTitle{
    width: 100%;
    display: flex;
    flex-wrap:wrap;
    position: relative;
    align-items: center;
    justify-content: center;
    img{
      width: 100%;
    }
    .TitleBg{
      width: 100%;
      height: 79px;
      // background: url('/images/pc/titilebg.png') center no-repeat;
      background-size: contain;
      text-align: center;
      position: relative;
      margin-top: 50px;
      .innerBoxText{
        font-size: 36px;
        font-weight: bold;
        color: #aa1638;
        // text-transform: uppercase;
        // letter-spacing: 1px;
        // padding-top: 44px;
      }
    }
  }

  .catContent {
      width: 1200px;
      margin: 0 auto;
      padding: 15px 0;

      .layer {
          font-size: 16px;
      }
  }
}
.catDetailback{
  background-color: #f2d5ce;

  .catCotent_text{
    min-height: 489px;
    width: 1200px;
    margin: 0 auto;

    padding-top: 155px;
    padding-left: 90px;
    box-sizing: border-box;
    .catCotent_title{
      font-size: 45px;
      // line-height: 1.3rem;
      color: #000;
      position: relative;
      margin-bottom: 25px;
      &::after{
        content: '';
        width: 30px;
        height: 5px;
        background-color: #000;
        position: absolute;
        left: 0;
        bottom: -10px;
      }
    }
    .layer{
      /deep/ p{
        font-size: 16px;
        color: #000;
        line-height: 30px;
      }
    }
  }
  .catContent{
    width: 100%;
    padding: 0;
    .background{
      width: 100%;
      height: 489px;
      background-attachment: fixed;
    }
  }
}
</style>
