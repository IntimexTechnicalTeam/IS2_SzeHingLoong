<template>
  <div class="PcVersion">
  <div class="productMain" >
    <div class="in_pdWindow_page_item" :style="styla" v-if="item">
      <div class="topWindowsImg imgbox" @click="click" v-if="item.AdditionalImages !== undefined && item.AdditionalImages.length > 0  "  v-on:mouseenter="mouseenterProd" v-on:mouseleave="mouseleaveProd">
        <img :src="item.AdditionalImages[0]" :data-key="item.Sku" @error="loadError"  class="addPic"/>
        <img :src="(item.Image?item.Image:item.Img_L?item.Img_L:item.Img)"  :style="imgStyla"  :data-key="item.Sku" @error="loadError" class="orgPic"/>
        <!-- <div class="shopMark">
            <div class="innerBox">
                <a  href="javascript:;"><i class="indexfav" v-bind:class="{'indexfav_hover':item.IsFavorite}"  v-on:click="addToFavorite(item)"></i><span v-on:click="addToFavorite(item)">{{$t('MyFavorite.MyFavorite')}}</span></a>
                <a  href="javascript:;" ><i class="showDetail" v-on:click="addCart(item)"></i><span v-on:click="addCart(item)">{{$t('home.ViewDetail')}}</span></a>
            </div>
        </div> -->
      </div>
      <div @click="click" class="topWindowsImg imgbox" v-else>

        <img :src="(item.Image?item.Image:item.Img_L?item.Img_L:item.Img)"  :style="imgStyla"  :data-key="item.Sku" @error="loadError" />

       </div>
        <div class="in_pdWindow_item_description">
            <router-link :to="'/product/detail/'+item.Sku" class="in_pdWindow_item_title" >{{item.Name}}</router-link >
            <div class="in_pdWindow_item_price">
              <inPrices :primePrices="item.ListPrice" :currentPrices="item.SalePrice" :currency="item.Currency" :DefaultListPrice="item.DefaultListPrice" :DefaultSalePrice="item.DefaultSalePrice" :DefaultCurrency="item.DefaultCurrency" size="small"></inPrices>
            </div>
        </div>
        <div class="addToCartItem">
          <span @click="addCart(item)">{{$t('product.addToCart')}}</span>
        </div>
    </div>
  </div>
  </div>
</template>
<script lang="ts">
import YouWouldLike from '@/model/youWouldLike';
import { Vue, Prop, Component } from 'vue-property-decorator';
@Component({ components: { inButton: () => import(/* webpackChunkName: "product" */ '@/components/base/pc/InsButton.vue'),
  inPrices: () => import(/* webpackChunkName: "product" */ '@/components/base/pc/InsPrices.vue') } })
export default class InsProductWindow extends Vue {
    private Enter:boolean = false;
    private newArray= [];
    @Prop() private item!:YouWouldLike;
    @Prop() private imgStyla!:string;
    @Prop() private styla!:string;
    buttonSubmit (item) {
      this.$router.push({
        path: '/product/detail',
        name: 'ProductsDetail',
        params: {
          id: item.Sku
        }
      });
    }
    mouseenterProd(event) {
      if (event) {
        $(event.target).find('.orgPic').stop().fadeOut();
        $(event.target).find('.addPic').stop().fadeIn();
      }
    }
    mouseleaveProd(event) {
      if (event) {
        $(event.path).eq(1).find('.orgPic').stop().fadeIn();
        $(event.path).eq(1).find('.addPic').stop().fadeOut();

        $(event.target).find('.orgPic').stop().fadeIn();
        $(event.target).find('.addPic').stop().fadeOut();
      }
    }
    click (e) {
      let target = e.target as HTMLElement;
      if (target.nodeName === 'IMG') { this.$router.push('/product/detail/' + target.dataset.key); };
    }
    addToFavorite (p) {
      if (p.IsFavorite) {
        this.$Api.member.removeFavorite(p.Sku).then((result) => {
          if (result.Succeeded) {
            p.IsFavorite = false;
            this.$message({
                message: this.$t('MyFavorite.RemoveSuccess') as string,
                type: 'error',
                customClass: 'messageboxNoraml'
              });
          }
        });
      } else {
        this.$Api.member.addFavorite(p.Sku).then((result) => {
          if (result.Succeeded) {
            p.IsFavorite = true;
            this.$message({
              message: this.$t('MyFavorite.AddSuccess') as string,
              type: 'success',
              customClass: 'messageboxNoraml'
            });
          } else {
            this.$router.push('/Account/login');
          }
        });
      }
    }
    addCart (val) {
      console.log(val.Sku);
      this.$Api.product.GetProduct(val.Sku).then((result) => {
        this.$Api.shoppingCart.addItem(val.Sku, 1, '1', '1', '1')
          .then(
            (result) => {
              this.$message({
                message: result.Message.Message as string,
                type: 'success',
                customClass: 'messageboxNoraml'
              });
            }).then(() => {
            this.$store.dispatch('setShopCart', this.$Api.shoppingCart.getShoppingCart());
          }).catch();
        // var a = result.PanelDetail.AttrList[0].length;
        // var b = result.PanelDetail.AttrList[1].length;
        // var c = result.PanelDetail.AttrList[2].length;
        // var d = result.PanelDetail.AttrList[3].length;
        // var e = result.PanelDetail.AttrList[4].length;
        // var f = result.PanelDetail.AttrList[5].length;
        // if (a || b || c || d || e || f) {
        //   this.$router.push('/product/detail/' + result.PanelDetail.Sku);
        // } else {

        // }
      });
    }

    loadError (e) {
      e.target.src = '/static/Image/proddef.jpg';
    }
}
</script>
<style lang="less">
.messageboxNoraml{
  z-index: 1000000000!important;
}
</style>
<style lang="less">
.PcVersion .in_pdWindow_item_price .currentPricesMain ,.in_pdWindow_item_price .primePricesMain{
  width: 100%;
  display: inline-block;
  text-align: center;
}
.PcVersion .in_pdWindow_item_price .currentPricesMain  .small {
  font-size: 18px;
  word-break: break-all;
  text-align: center;
  color: #cd0909;
  display: inline-block;
}
// .PcVersion .in_pdWindow_item_price .currentPricesMain .small:nth-child(2) {
//     font-size: 1.4rem;
//     color: #cd0909;
//     display: inline-block;
// }
.PcVersion .in_pdWindow_item_price .primePricesMain  .small {
  font-size: 14px;
  word-break: break-all;
  text-align: center;
  color: #999;
  display: inline-block;
  text-decoration:line-through;
}
.PcVersion .in_pdWindow_item_price{
  height: 40px;
}
// .PcVersion .in_pdWindow_item_price .primePricesMain .small:nth-child(2) {
//   font-size: 1rem;
//   color: #999;
//   display: inline-block;
// }
// .productMain:hover .in_pdWindow_page_item img {
//     // border: 1px solid #cd0909;
// }
.productMain:hover .in_pdWindow_item_title {
    transform: translateY(-3px);
    color: #cd0909!important;
}
</style>
<style lang="less" scoped>
.in_pdWindow_page_item {
  box-sizing: border-box;
  cursor: pointer;
  width: 100%;
  // min-height: 450px;
  transition: all 1s;
  background: #fff;
  // overflow: hidden;
  position: relative;
  // box-shadow: 0 0 5px 0 #d4d5d1;
  border:1px solid #fff;
  padding-bottom: 20px;
  text-align: center;
  .imgbox{
    width: 100%;
    height: 250px;
    position: relative;
    text-align: center;
    display: flex;
    justify-content: center;
    align-content: center;
    img{
      position: absolute;
      top: 0;
      max-width: 100%;
      max-height: 100%;
    }
}
  // border: 1px solid #000;
  .addToCartItem {
    width: 100%;
    display: inline-block;
    left: 0px;
    bottom: -35px;
    // background: #112346;
    background: rgba(170,22,56,.7);
    position: absolute;
    height: 35px;
    line-height: 35px;
    text-align: center;
    transition: opacity .3s;
    font-size: 18px;
    text-transform: uppercase;
    opacity: 0;
    span {
      width: 100%;
      display: inline-block;
      color: #fff;
    }
  }
  &:hover {
    // border:1px solid #cd0909;
    .addToCartItem {
      // bottom: 0px;
      opacity: 1;
    }
  }
}

.imgbox:hover .shopMark{
    bottom: 0px;
}
.imgbox .shopMark{
    position: absolute;
    left: 0px;
    bottom: -100%;
    right: 0px;
    height:100%;
    background:rgba(0,0,0,.5);
    transition: all .3s;
    display: flex;
    align-items: center;
    justify-content: center;
    .innerBox{
      width: 100%;
      display: inline-block;
    }
}
.imgbox .shopMark a{
    text-align: center;
    display: block;
    transition: 0.3s all;
}
.imgbox .shopMark a:hover{
  transform: translateY(-3px);
}
.imgbox .shopMark a:hover span{
    text-decoration: underline;
}
.imgbox .shopMark a:nth-child(1){
    // padding-top: 60px;
    padding-bottom: 20px;
}
.imgbox .shopMark a span{
    color: #FFF;
    font-size: 16px;
    margin-top: 5px;
    display: block;
}
.imgbox .shopMark i{
    width: 35px;
    height: 35px;
    display: block;
    margin: 0 auto;
}
.imgbox .shopMark .indexfav{
    background: url(/images/pc/index_42.png) no-repeat center center;
    background-size: 100%;
}
.imgbox .shopMark .indexfav_hover{
    background: url(/images/pc/index_42hover.png) no-repeat center center!important;
    background-size: 100%;
}
.imgbox .shopMark .showDetail{
    background: url(/images/pc/viewDetail.png) no-repeat center center;
    background-size: 100%;
}

.imgbox img{
    // width: 100%;
    // border-radius:0px;
    // // border:1px solid #020202;
    // transition: border all 1s;
    // box-sizing: border-box;
}
.imgbox img:hover{
    // border:1px solid #e02533;
}
.in_pdWindow_page_item img {
  box-sizing: border-box;
  cursor: pointer;
  // border: 1px solid #cdcdcd;
  border-radius:0px;
}
// .height_line {
//   border: 1px solid black !important;
// }
.in_pdWindow_item_title {
    font-size: 16px;
    width: 90%;
    margin: 0 auto;
    word-break: break-all;
    text-align: center;
    color: #0b0b0b;
    display: inline-block;
    text-align: center;
    line-height: 25px;
    overflow: hidden;
    display: -webkit-box;
    -webkit-line-clamp: 1;
    -webkit-box-orient: vertical;
    word-break: break-word;
    margin-top: 10px;
    margin-bottom: 10px;
    transition: 0.3s all;
}
.in_pdWindow_item_code {
  margin-top: 1rem;
  color: #999999;
  text-align: center;
}
</style>
