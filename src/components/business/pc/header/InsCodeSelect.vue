<template>
  <div class="main-code">
    <select id="code-select" v-model="codeId" @change="changeCurrency(codeId)">
      <option value="1" style="color:#000">HKD</option>
      <option value="2" style="color:#000">USD</option>
      <option value="3" style="color:#000">RMB</option>
    </select>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component
export default class InsCodeSelect extends Vue {
  private codeId: number = 1;
  changeCurrency (val) {
    this.$Api.member.setCurrency(val).then((result) => {
      this.$message({
        message: this.$t('changeCurSuccess') as string,
        type: 'success'
      });

      this.$Storage.set('currency', result.Currency.Id);
      this.Reload();
    }).catch((error) => {
      console.log(error);
    });
  }

  created() {
    this.codeId = this.$Storage.get('currency');
  }

  // LoadData () {
  //   this.$Api.shoppingCart.LoadData().then((result) => {
  //     this.codeId = result.Currency.Id;
  //   });
  // }
  mounted () {
    // this.LoadData();
  }
}
</script>
<style scoped lang="less">
.main-code {
  display: inline-block;
  margin-left: 20px;
  width: 55px;
  select {
    width: 100%;
    font-size: 16px;
    background: none;
    color: #666666;
    border: none;
    outline: none;
    height: 30px;
    background: transparent url('/images/pc/arrow-down-back.png')  90% 50% no-repeat;
    background-size: 11px;
    appearance: none;
    -moz-appearance: none;
    -webkit-appearance: none;

  }
}
</style>
