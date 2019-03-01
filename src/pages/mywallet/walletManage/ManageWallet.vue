<template>
  <div id="ManageWallet">
    <!-- import-weidex-Btn -->
    <div class="imp-wallet">
      <div class="imp-btn">
        <img style="width:22px;height:22px;" :src="importWalletImg"  @click="click"/>
        <span>导入文件</span>
        <!-- <input type="file" @change="importWallet"> -->
      </div>
    </div>
    <!-- Dailog for noActive Wallet -->
    <!-- Dailog Befor Import -->
    <div style="display:flex;margin:10px 0;">
      <div style="width:270px;height:30px;border-radius:4px;background-color:#439187;text-align:center;line-height:30px;">更改交易密码</div>
      <div style="width:270px;height:30px;border-radius:4px;background-color:#D5832C;text-align:center;line-height:30px;">导出weidex文件</div>
    </div>
     <!-- body - wallet list -->
    <ul class="kindWallet">
      <li v-for="(item,idx) in list" :key="idx">
        <div style="width:100%;height:50px;margin:5px 0;background-color:#232B31;display:flex;align-items:center;position:relative">
          <div :style="{background:'url('+item.titleBg+')no-repeat',backgroundSize:'100%'}" style="width:70px;height:25px;">{{item.name}}</div>
          <span class="operateWalletBtn" style="width:31px;height:20px;line-height:20px;background-color:#346BA7;font-size:12px;border-radius:4px;text-align:center;" >添加</span>
        </div>
      </li>
    </ul>
    <!--Clear wallet cache -->
    <div style="display:flex;padding-bottom:0.16rem;" @click="clear">
      <button class="clearWalletCache" >
        <span><img :src="clearWalletImg" style="width:13px;height:13px"  ></span>
        清除钱包缓存
      </button>
      <!-- <clear-wallet-cache ref="clickClear" v-on:close="clickClear"></clear-wallet-cache> -->
    </div>
    <!-- some Tips Info -->
    <div style="font-size:12px;padding: 0 0.1rem;">
      <img :src="reminderImg" style="width:12px;height:12px;margin-bottom:3px;">
      <span style="color: #f37028;">温馨提示</span>
      <p style=" margin: 0;">* 修改功能会覆盖当前钱包信息，请在修改前确定已保存好钱包密钥。</p>
      <p style=" margin: 0;">* 查看功能可以看到用户的钱包地址以及密钥，请在安全环境下使用。</p>
      <p style=" margin: 0;">* 添加功能可以添加非SWTC钱包，方便用户跨链充提币操作。</p>
    </div>
  </div>
</template>

<script>
import importWallet from "../../../images/import_wallet.png";
// import walletActive from "@/components/walletActive";
import modifyPsw from "../../../images/modifyPsw.png";
import swtBg from "../../../images/manageWallet_swt_Bg.png";
import ethBg from "../../../images/manageWallet_eth_Bg.png";
import stmBg from "../../../images/manageWallet_stm_Bg.png";
import callBg from "../../../images/manageWallet_call_Bg.png";
import moacBg from "../../../images/manageWallet_moac_Bg.png";
import lookWalletImg from "../../../images/lookWallet_Title.png";
import clearWalletImg from "../../../images/clearWallet.png";
import reminderImg from "../../../images/asset_Reminder.png";
import modifyAdressImg from "../../../images/editx24_click.png";
import { mapState, mapMutations } from "vuex";
import { TEST } from "../../../store/mutation-types";
export default {
  name: "ManageWallet",
  data() {
    return {
      importWalletImg: importWallet,
      noActivWalletTip: false,
      showImportDialog: false,
      importDialogTitle: "导入钱包",
      modifyPswImg: modifyPsw,
      lookWalletImg,
      reminderImg,
      modifyAdressImg,
      clearWalletImg,
      active: false,
      noActivWalletTip: false,
      showClearDialog: false,
      swtBg,
      ethBg,
      stmBg,
      callBg
    };
  },
  // components: {
  //   walletActiveTip: walletActive
  // },
  computed: {
    list() {
      let list = [
        {
          titleBg: swtBg,
          name: "swt",
          type: "swt"
        },
        {
          titleBg: ethBg,
          name: "eth",
          type: "eth"
        },
        {
          titleBg: stmBg,
          name: "stm",
          type: "stm"
        },
        {
          titleBg: callBg,
          name: "call",
          type: "call"
        },
        {
          titleBg: moacBg,
          name: "moac",
          type: "moac"
        },
        {
          titleBg: swtBg,
          name: "biz",
          type: "biz"
        }
      ];
      return list;
    },
    ...mapState(["test"])
  },
  methods: {
    ...mapMutations([TEST]),
    click() {
      console.log(this.test);
      // this[TEST](Math.random());
      const logs = wx.getStorageSync("Wallet") || [];
      // logs.unshift(Date.now())
      // wx.setStorageSync('Wallet', "jcc")
      console.log(logs);
      wx.setStorageSync("Wallet", "jcc");
      console.log("添加钱包");
    },
    clear() {
      wx.removeStorageSync("Wallet");
      console.log("清除缓存");
      wx.reLaunch({
        url: "../../../main?id=0"
      });
    }
  }
};
</script>


<style lang="css" scoped>
#ManageWallet {
  font-family: "simhei";
  color: #fff;
  padding: 0.1rem 0.1rem 0.88rem 0.1rem;
  background-color: #151c21;
}
.imp-wallet {
  padding: 0 0.1rem;
}
.imp-btn {
  height: 1rem;
  margin: 0.1rem 0 0.18rem 0;
  line-height: 1rem;
  text-align: center;
  border: 0.02rem dashed #38434f;
  position: relative;
}
img {
  position: relative;
  top: 0.1rem;
}
input {
  width: 93%;
  height: 80%;
  position: absolute;
  top: 0.1rem;
  left: 0.2rem;
  opacity: 0;
}
.operateWalletBtn {
  position: absolute;
  right: 0.3rem;
  top: 0.3rem;
}
.clearWalletCache {
  height: 36px;
  line-height: 36px;
  width: 100%;
  font-size: 12px;
  color: #e6e6e6;
  background-color: #3d5e8e;
  border: none;
  border-radius: 3px;
}
</style>
