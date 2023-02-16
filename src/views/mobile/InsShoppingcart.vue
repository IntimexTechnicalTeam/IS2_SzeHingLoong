<template>
  <div class="shoppingcart_warrper">
    <!--main-content-->
        <div class="shoppingcart_header TitleBg">
          <div class="innerBox">
            {{$t('Shoppingcart.ShoppingcartTitle')}}
          </div>
        </div>
        <div class="ShoppingCartItem_warpper"  v-for="(one,index) in items" :key="index">
            <div class="shoppingcart_item_image">
                <a class="product-img" v-bind:href="'/product/Detail/'+one.Product.Sku">
                    <img v-bind:src="one.Product.Img_M" alt />
                  </a>
            </div>
            <div class="shoppingcart_item_detail">
                <div class="shoppingcart_item_name">{{one.Product.Name}}</div>
                <!-- <div class="shoppingcart_item_code">{{one.Product.Code}}</div> -->
                <!-- <div class="shoppingcart_item_attr">
                      <span v-if="one.AttrName1">{{one.AttrTypeName1}}：{{one.AttrName1}}</span>&nbsp;
                      <span v-if="one.AttrName2">{{one.AttrTypeName2}}：{{one.AttrName2}}</span>&nbsp;
                      <span v-if="one.AttrName3">{{one.AttrTypeName3}}：{{one.AttrName3}}</span>&nbsp;
                </div> -->
                <div class="shoppingcart_item_price">
                    <div>{{Currency.Code}} {{(one.Product.SalePrice) | PriceFormat}}</div>
                </div>
                <div class="shoppingcart_item_qty">
                  <div class="common-num">
                      <a
                        class="reduce-num"
                        href="javascript:;"
                        v-on:click=" minusQty(one,one.Id,$event);"
                      >-</a>
                      <div class="num-content">
                        <input
                          class="input-text"
                          type="text"
                          data-num="1"
                          disabled
                          v-model="one.Qty"
                          v-on:change="updateQty(one,one.Id,$event)"
                        />
                      </div>
                      <a class="add-num" href="javascript:;" v-on:click="plusQty(one,one.Id,$event);" :class="{'disabled':one.IsAdd}">+</a>
                      </div>
                </div>

            </div>
            <div class="close"  v-on:click="removeItem(index)">
                <i class="el-icon-close"></i>
            </div>
        </div>
        <div>
          <div class="shoppingcart_total1"><span>{{$t('Shoppingcart.SubTotal')}}:</span>&nbsp;&nbsp;{{Currency.Code}} {{(totalAmount) | PriceFormat}}</div>
          <div class="shoppingcart_total"><ElButton type="success" @click="submit"><span style="font-size:1.5rem;">{{ $t('Shoppingcart.Checkout') }}</span></ElButton></div>
        </div>
    <!--main-content-->
  </div>
</template>
<script lang='ts'>
import { Component, Prop, Vue, Watch } from 'vue-property-decorator';
import ShopCart from '../../model/ShopCart';
import ShopCartItem from '../../model/shopCartItem';
import Currency from '../../model/currency';
import { Button as ElButton } from 'element-ui';
class Update {
  itemId!: string;
  qty!: number;
  constructor (itemId:string, qty:number) {
    this.itemId = itemId;
    this.qty = qty;
  }
}
@Component({ components: { ElButton } })
export default class InsShoppingcart extends Vue {
  private ShoppingCart:ShopCart = new ShopCart();
  prodcutSrc: string = require('@/assets/Images/270_b.jpg');
  step: number = 1;
  totalAmount: number = 0;
  // itemsAmount: number = 0;
  // ItemsTaxAmount: number = 0;
  Currency: Currency = new Currency();
  MaxQty: number = 20;
  // cartItems: any[] = [];
  // currentCode: any = '';
  items: any[] = [
  ];
  itemQty:number=0;
  private UpdateQueQue:Update[] = [];
  isAdd:boolean = false;
  mounted () {
    // this.loadItems();
  }
  created () {
    this.load().then(() => { this.$HiddenLayer(); });
  }
  load () {
    let load = this.$Api.shoppingCart.getShoppingCart().then((result) => {
      this.ShoppingCart = result.ShopCart;
      this.Currency = result.ShopCart.DefaultCurrency;
      this.items = result.ShopCart.Items;
      this.items.forEach(v => {
        this.$set(v, 'IsAdd', false);
      });
      if (this.ShoppingCart.Items.length === 0) this.$Confirm(this.$t('Message.Message'), this.$t('Shoppingcart.None'), () => { this.$router.push('/product/search/-'); }, () => { this.$router.push('/'); });
    });
    this.loadItems();
    return load;
  }
  loadItems () {
    var _this = this;
    var itemsprice = 0;
    var Currencys;
    var itemQ;
    _this.items.forEach(element => {
      itemsprice += element.Product.SalePrice * element.Qty;
      Currencys = element.Product.Currency.Code;
      itemQ = element.Qty;
    });
    // _this.Currency = Currencys;
    _this.totalAmount = itemsprice;
    _this.itemQty = itemQ;
  }
  @Watch('items', { deep: true })
  onItemsChange (o, n) {
    this.loadItems();
  }
  removeItem (one) {
    // this.loadItems();
    let item:ShopCartItem = this.items.splice(one, 1)[0];
    this.$Api.shoppingCart.removeItem(item.Id).then(result => {
      this.$store.dispatch('setShopCart', this.$Api.shoppingCart.getShoppingCart());
      if (this.ShoppingCart.Items.length === 0) this.$Confirm(this.$t('Message.Message'), this.$t('Shoppingcart.None'), () => { this.$router.push('/product/search/-'); }, () => { this.$router.push('/'); });
    });
  }
  next () {
    // if (!this.items || this.items.length === 0) {
    //     showInfo('<%=Resources.Caption.CartEmpty%>');
    //     return false;
    // }
  }
  minusQty (one, id, event) {
    let _this = this;
    one.Qty--;
    if (one.Qty < 1) {
      one.Qty = 1;
    }
    this.Shake(() => {
      this.$Api.shoppingCart.updateItemQty(id, one.Qty).then((result) => {
            this.$store.dispatch('setShopCart', this.$Api.shoppingCart.getShoppingCart());
            one.IsAdd = false;
      });
    }, 500);
  }
  plusQty (one, id, event) {
    var a = one.Qty;
    let _this = this;
    one.Qty++;
    if (one.IsAdd === false) {
        one.IsAdd = true;
      setTimeout(() => {
             _this.$Api.shoppingCart.updateItemQty(id, one.Qty).then((result) => {
               if (result.Message.Succeeded) {
                 _this.$store.dispatch('setShopCart', this.$Api.shoppingCart.getShoppingCart());
                 one.IsAdd = false;
               } else {
                 one.Qty = a;
                  this.$message({
                      message: result.Message.Message as string,
                      type: 'success',
                      customClass: 'messageboxNoraml'
                });
               }
            });
      }, 500);
    }
  }
  updateQty (one, id, event) {
    let _this = this;
    if (one.Qty < 1) {
      one.Qty = 1;
    }
    // if (one.Qty >= this.MaxQty) {
    //   one.Qty = this.MaxQty;
    // }
  }
  submit () {
    let temp = {};
    let item:Update;
    let waittingList = [Promise.resolve('head')];
    while (this.UpdateQueQue.length !== 0) {
      item = this.UpdateQueQue.shift() as Update;
      temp[item.itemId] = item.qty;
    }
    Object.keys(temp).forEach((element) => {
      waittingList.push(this.$Api.shoppingCart.updateItemQty(element, temp[element]));
    });
    Promise.all(waittingList).then((result) => {
      // this.$router.push('/account/checkout');
      if (this.$Storage.get('isLogin') === 1) this.$router.push('/account/checkout');
      else {
        // this.$Login(() => { this.$ShowLayer(); this.load().then(() => { this.$HiddenLayer(); }); });
        this.$router.push('/account/login?returnurl=/account/checkout');
      }
    });
  }
}
</script>
<style lang="less">
.ShoppingCartItem_warpper .el-input-number {
    position: relative;
    display: inline-block;
    width: 8rem;
    border: none;
}
</style>
<style scoped lang='less'>
/*我的最爱*/
  .disabled {
      pointer-events: none;
      filter: alpha(opacity=50); /*IE滤镜，透明度50%*/
      -moz-opacity: 0.5; /*Firefox私有，透明度50%*/
      opacity: 0.5; /*其他，透明度50%*/
  }
#main-content {
  width: 90%;
  margin: 0 auto;
}
/*购物流程*/

.ShoppingCartItem_warpper{
    display: flex;
    flex-wrap: nowrap;
    padding-top: 1rem;
    padding-bottom: 1rem;
    border-bottom: 1px solid #eee;
    position: relative;
    width: 94%;
    margin: 0 auto;
    .close{
        position: absolute;
        top: 50%;
        right: 0.5rem;
        transform: translateY(-50%);
        i{
            font-size: 1.2rem;
        }
        .el-icon-close{
          width: 2rem;
          height: 2rem;
          line-height: 2rem;
          text-align: center;
          background-color: #cccccc;
          color: #fff;
          border-radius: 50%;
        }
    }
}
.shoppingcart_item_image{
    // margin: 0 0 0 1rem;
}
.shoppingcart_item_image img{
    width: 8rem;
    height: 8rem;
    display: block;
    object-fit: cover;
    object-position: 50% 50%;
}
.shoppingcart_item_detail{
    margin: 0 0 0 1rem;
    margin-right: 2rem;
}
.shoppingcart_item_name,
.shoppingcart_item_code,
.shoppingcart_item_attr,
.shoppingcart_item_price{
    line-height: 2rem;
    font-size: 1.2rem;
    // width: 10rem;
    color: #666666;
    font-family: 'Poppins-Light';
}
.shoppingcart_item_price >div{
    font-size: 1.2rem;
    color:#d92526;
}
.shoppingcart_item_qty{
    display: flex;
    .qty_count{
        line-height: 26px;
        font-size: 1.2rem
    }
}
.shoppingcart_item_attr{
    span{
        margin-right: 1rem;
    }
}
.shoppingcart_item_price{
    display: flex;
    flex-wrap: nowrap;
    justify-content: space-between;
    margin-bottom: 0.8rem;
}
.shoppingcart_warrper{
    background-color: white;
    padding: 2rem 0;
    border-radius: .5rem;
    // min-height: calc(100vh - 402px);
    .shoppingcart_header{
        // font-size: 2rem;
        // line-height: 5rem;
        // text-align: center;
        // font-weight: 700;
        // border-bottom: 1px solid #eee;
        margin-bottom: 2rem;
    }
    .TitleBg{
      width: 100%;
      background-size: contain;
      text-align: center;
      position: relative;
      .innerBox{
        font-size: 2rem;
        color: #aa1638;
        letter-spacing: 1px;
        padding-top: 2rem;
        font-weight: 700;
      }
    }
}
.shoppingcart_total{
    text-align: right;
    padding: 1rem;
    .el-button{
      width: 100%;
      height: 3rem;
      // background: url('/images/mobile/CONTACT_btn.png') no-repeat center;
      background-color: #aa1638;
      background-size: cover;
      padding: 0;
      text-transform: uppercase;
      letter-spacing: 2px;
    }
}
.shoppingcart_total1{
    font-size: 1.4rem;
    text-align: right;
    padding: 1rem;
    color:#d92526;
    span{
      color: #999999;
      font-size: 1.4rem;
    }
}
.num-content{
    float: left;
}
.num-content .input-text {
  display: inline-block;
  width: 38px;
  height: 30px;
  line-height: 30px;
  text-align: center;
  border: none;
  color: #999999;
  outline: none;
  border-left: 1px solid #e0e0e0;
  border-right: 1px solid #e0e0e0;
  background-color: #ffffff;
}
.common-num {
  display: inline-block;
  border: 1px solid #e0e0e0;
  border-radius: 3px;
}
.common-num a{
    float: left;
    width: 30px;
    height: 32px;
    line-height: 28px;
    text-align: center;
    font-size: 20px;
    color: #999999;
    background-color: #f5f5f5;
    font-weight: bold;
}
</style>
