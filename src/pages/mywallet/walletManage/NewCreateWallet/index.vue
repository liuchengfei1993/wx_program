<template>
  <div style="color:#fff;">
     <!-- 新建钱包 head_title -->
      <div style="text-align: center;background-color: #29313e;" @click="add">
        <span>钱包名称：SWTC钱包</span>
      </div>
      <!-- step-1 -->
      <div style="color:red;">
        <div>数字资产去中心化交易平台，不保存您的钱包数据，特别是钱包密钥，</div>
        <p>一旦丢失，无法找回，请复制保存并抄写<span>务必妥善保管!!</span></p>
        <ul v-for="(item ,index) in items" :key="index">
          <li>{{item.name}}</li>
        </ul>
      </div>
  </div>
</template>

<script>
const Wallet = require("jcc_jingtum_lib").Wallet;
export default {
  data() {
    return {
      wallet: {},
      items: []
    };
  },
  mounted() {
    var that = this;
    wx.request({
      url: "http://120.78.73.52:3389/123", // 仅为示例，并非真实的接口地址
      header: {
        "content-type": "application/json" // 默认值
      },
      success(res) {
        that.items = res.data;
        console.log(that.items[0]);
      }
    });
    // console.log(this.items);
  },
  methods: {
    add() {
      let res = this.items;
      console.log(res[0].name);
    }
  }
};
</script>

<style scoped>
</style>
