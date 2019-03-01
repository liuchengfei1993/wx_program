<template>
    <div style="background-color:#232B34;width:100%;" :style="windowHeight">
      <div style="width: 80%;padding:20px 20px 0 20px;">
        <p style="width: 100%;color:#b9cadd" @click="add">钱包名称：SWTC钱包</p>
        <div style="padding-top:15px;display:flex;">
          <div style="width: 50%;height: 35px;line-height:35px;white-space: nowrap;text-align: center;border-radius: 17px 0 0 17px;border:1px solid #fff;color:#fff" :style="background_color1" @click="secretkey">通过秘钥导入</div>
          <div style="width: 50%;height: 35px;line-height:35px;white-space: nowrap;text-align: center;border-radius: 0 17px 17px 0;border:1px solid #fff;color:#fff" @click="weidex">通过weidex文件</div>
        </div>
      </div>
       <div v-show="selectImportType===1">
         <input type="password" placeholder="请输入SWTC钱包交易密匙" style="background-color:#44546D;width:80%;height:30px;border-radius:4px;margin:15px" />
         <input type="password" placeholder="请设置交易密码" style="background-color:#44546D;width:80%;height:30px;border-radius:4px;margin:15px;" />
         <input type="password" placeholder="请确认交易密码" style="background-color:#44546D;width:80%;height:30px;border-radius:4px;margin:15px;" />
        </div>
        <div v-show="selectImportType===2" style="position:relative;margin:0.4rem 0;color:#768db0;;overflow:hidden;" >
          <input type="file"  @click="readFile" placeholder="请导入weidex文件" style="background-color:#44546D;width:80%;height:30px;border-radius:4px;margin:15px" />
        </div>
         <ul style="color:#fff;font-size:12px;">
            <li><img :src="tipsIcon" style="width:10px;height:10px">温馨提示</li>
            <li>* 请仔细核对钱包信息。</li>
            <li>* 导入的SWTC钱包将会出现在钱包管理里面。</li>
            <li>* 设置交易密码方便用户授权，非常重要！</li>
            <li>* 平台不接触不保存也无法帮您找回交易密码，请务必牢记！</li>
          </ul>
          <button style="width:80%;margin:5% 10% 0 10%;background-color:#389BE6" @click="add">确认</button>
          <button v-show="selectImportType===2" style="width:80%;margin:5% 10% 0 10%;background-color:#389BE6" @click="scanCode">扫一扫导入钱包</button>
          <button v-show="selectImportType===1" style="width:80%;margin:5% 10% 0 10%;background-color:#389BE6" @click="scanCode">扫一扫输入密匙</button>
    </div>
  
</template>
<script>
import tipsIcon from "../../../../images/asset_Reminder.png";
// import { readFile } from 'fs';

export default {
  data() {
    return {
      windowHeight: "",
      background_color: "",
      selectImportType: 1,
      tipsIcon: "",
      background_color1: "background-color:#389BE6",
      path: ""
    }
  },
  mounted() {
    var that = this;
    let res = wx.getSystemInfoSync().windowHeight;
    that.windowHeight = "height:" + res + "px;";
    console.log(that.windowHeight)
  },
  methods: {
    add() {
      wx.getSystemInfoSync({
        success(res) {
          console.log(res.windowHeight)
        }
      });
    },
    background_color() {},
    //读取文件
    readFile(e) {
      var that = this;
      wx.chooseMessageFile({
        count: 1,
        type: "image",
        success(res) {
          that.path = res.tempFiles[0].path;
          console.log(that.path)
        }
      })
    },
    add() {
      console.log(this.path)
      wx.getFileInfo({
        filePath: this.path,
        digestAlgorithm: "md5",
        success: function(res) {
          console.log(res);
          wx.setStorageSync("Wallet", res.digest);
          wx.reLaunch({
            url: "../../../mywallet/main?id=1"
          })
        },
        fail: function(err) {
          console.log("调用失败");
        }
      })
    },
    //扫一扫
    scanCode() {
      wx.scanCode({
        onlyFromCamera: true,
        success(res) {
          console.log(res);
        }
      });
    },
    secretkey() {
      this.selectImportType = 1;
    },
    weidex() {
      this.selectImportType = 2;
    }
  }
};
</script>
<style>

</style>
