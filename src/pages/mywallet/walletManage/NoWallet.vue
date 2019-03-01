<template>
  <div style="background-color: #161b21;" :style="windowHeight">
    <!-- 无钱包提示 -->
    <div style="color: #ff0000;text-align: center;padding-top: 40px;"><img :src="noneicon" style=" width:15px;height:15px;"/>
      <span style=" padding-left: 10px;position: relative;top:-4px;">未检测到您的钱包</span>
    </div>
    <div style=" margin: 0 40px;color: #b9cadd;text-align: center;padding-top: 20px;padding-bottom: 40px;"><img :src="noWalletTitle" style="width:0.3px;height:0.3px;" />
      <span style=" color: #f37028;">必须拥有一款SWTC的钱包您才可以参与威链平台交易， 请新建或导入钱包
      </span> 
    </div>
    <!-- 新建钱包btn -->
      <div style="height:100px;line-height:100px;width:90%;text-align:center;border:2px dashed  #38434f;margin:15px;">
        <div  @click="newCreateWallet">
          <img :src="addwallet" style="width:15px;height:15px;"/>
          <span style="margin-left:10px;color: #e6e6e6;top: -1px;">新建SWTC钱包</span>
        </div>
      </div>
    <!-- 导入钱包btn -->
      <div style="height:100px;line-height:100px;width:90%;text-align:center;border:2px dashed  #38434f;margin:15px;">
        <div  @click="addWallet">
          <img :src="importWallet" style="width:15px;height:15px;"/>
          <span style="margin-left:10px;color: #e6e6e6;top: -1px;">导入SWTC钱包</span>
        </div>
      </div>
  </div>
</template>
<script>
import noneicon from "../../../images/asset_nowallet.png";
import noWalletTitle from "../../../images/asset_nowallet_tip.png";
import addwallet from "../../../images/add_wallet.png";
import importWallet from "../../../images/import_wallet.png";
export default {
  name: "NoWallet",
  data() {
    return {
      windowHeight:"",
      noneicon,
      noWalletTitle,
      addwallet,
      importWallet
    };
  },
   mounted(){
    var that = this;
    let res = wx.getSystemInfoSync().windowHeight;
    that.windowHeight = "height:" + res +"px;";
    console.log(that.windowHeight);
  },
  methods:{
    newCreateWallet(){
      console.log('新建钱包');
      wx.redirectTo({
        url: 'walletManage/NewCreateWallet/main'
      })
    },
    addWallet(){
    console.log('导入钱包');
    wx.redirectTo({
      url:'walletManage/addWallet/main'
    })
  }
  },
}
</script>
<style>
</style>
