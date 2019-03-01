<template>
  <scroller style="margin-top:0.9rem;">
    <NoWallet v-if="!show"></NoWallet>
    <ManageWallet v-if="show"></ManageWallet>
  </scroller>
</template>

<script>
import NoWallet from "./walletManage/NoWallet.vue";
import ManageWallet from "./walletManage/ManageWallet.vue";
import { mapState } from "vuex";
import { TEST } from "../../store/mutation-types";

export default {
  data() {
    return {
      show: ""
    };
  },
  name: "wallet",
  components: { NoWallet, ManageWallet },
  onLoad(option) {
    var that = this;
    console.log(option.id);
    if (option.id === "1" ||  wx.getStorageSync("Wallet") !== "") {
      that.show = true;
      console.log(that.show + "第一个判断");
    } else {
      that.show = false;
      console.log(that.show + "第二个判断");
    }
  },
  ...mapState([TEST])
};
</script>

