<template>
  <div id=weChatRunning :style="getWindow" >
    <!-- <img :src="bg" style="width:100%;height:100%;position:absolute;z-index:-1"> -->
    <div style="color:#AFAFAF;width:100%;height:40px;line-height:40px;text-align:center;border-bottom:1px solid #AFAFAF" @click="getRunningStep">步数排行榜</div>
    <div style="display:flex;padding:5px;border-bottom:1px solid #AFAFAF">
      <div style="width:50%;height:10px;line-height:10px;color:#AFAFAF;text-align:center">时间</div>
      <div style="width:50%;height:10px;line-height:10px;color:#AFAFAF;text-align:center">步数</div>
    </div>
    <ul>
      <li v-for="(item,idx) in steps" :key="idx" style="display:flex;color:#AFAFAF;text-align:center">
        <div style="width:50%;height:30px;line-height:30px;color:#AFAFAF;text-align:center;border-bottom:1px solid #AFAFAF">{{item.timestamp}}</div>
        <div style="width:50%;height:30px;line-height:30px;color:#AFAFAF;text-align:center;border-bottom:1px solid #AFAFAF">{{item.step}}</div>
      </li>
    </ul>
  </div>
</template>

<script>
// import bg from "../../images/bg750.png";
var WXBizDataCrypt = require("./WXBizDataCrypt");

export default {
  data() {
    return {
      // bg,
      getWindow: "",
      steps: []
    };
  },
  mounted() {
    var that = this;
    let height = wx.getSystemInfoSync().windowHeight;
    let width = wx.getSystemInfoSync().windowWidth;
    that.getWindow = "height:" + height + "px;width:" + width + "px;";
  },
  created() {},
  methods: {
    getRunningStep() {
      var that = this;
      //验证登录，拿到res.code
      wx.login({
        success: function(res) {
          var appid = "wx43c1c9647ca0209f";
          var secret = "67b546c0e5af6496ec5b3b2e747ef25a";
          //向微信服务器发送请求
          if (res.code) {
            wx.request({
              url:
                "https://api.weixin.qq.com/sns/jscode2session?appid=" +
                appid +
                "&secret=" +
                secret +
                "&js_code=" +
                res.code +
                "&grant_type=authorization_code",
              header: {
                "content-type": "json"
              },
              success: function(res) {
                var session_key = res.data.session_key;
                var openid = res.data.openid;
                //调取微信运动接口并解析数据
                wx.getWeRunData({
                  success(res) {
                    var encryptedData = res.encryptedData;
                    var iv = res.iv;
                    var pc = new WXBizDataCrypt(appid, session_key);
                    var data = pc.decryptData(encryptedData, iv);
                    var step = data.stepInfoList;
                    that.handleStep(step);
                    that.steps = step;
                  },
                  fail: function(res) {
                    wx.showModal({
                      title: "提示",
                      content:
                        "您尚未开通微信运动，请关注“微信运动”公众号后重试",
                      showCancel: false,
                      confirmText: "知道了"
                    });
                  }
                });
              }
            });
          }
        }
      });
    },
    handleStep(data) {
      var handledata = data.reverse();
      console.log(handledata);
      for (var i = 0; i < 1; i++) {
        var time = new Date(handledata[0].timestamp*1000);
        var date =
          time.getFullYear() +
          "年" +
          (time.getMonth() +
          1) +
          "月" +
          time.getDate() +
          "日";
        console.log(date);
      }
    }
  }
};
</script>

<style>
</style>
