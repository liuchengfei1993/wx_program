<template>
  <div id="main">
    <div v-if="isHide">
        <div class='header'>
            <image></image>
        </div>
        <div class='content'>
            <div>申请获取以下权限</div>
            <text>获得你的公开信息(昵称，头像等)</text>
        </div>
        <button class='bottom' type='primary'  open-type="getUserInfo" @getuserinfo="bindGetUserInfo">获取权限</button>
    </div>
    <div v-else>
      <div>我的首页内容</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isHide: true
    };
  },
  mounted() {
    var that = this;
    //查看是否授权
    wx.getSetting({
      success: function(res) {
        console.log(res.authSetting);
        //已经授权了
        if (res.authSetting["scope.userInfo"]) {
          that.isHide = false;
          console.log(res.authSetting);
        }
      }
    });
  },
  methods: {
    bindGetUserInfo(e) {
      console.log(e);
      const that = this;
      if (e.mp.detail.userInfo) {
        console.log("用户按了允许授权按钮");
        that.isHide = false;
      } else {
        //用户按了拒绝按钮
        console.log("用户按了拒绝按钮");
      }
    }
  }
};
</script>

<style scoped>
.header {
  margin: 90rpx 0 90rpx 50rpx;
  border-bottom: 1px solid #ccc;
  text-align: center;
  width: 650rpx;
  height: 300rpx;
  line-height: 450rpx;
}

.header image {
  width: 200rpx;
  height: 200rpx;
}

.content {
  margin-left: 50rpx;
  margin-bottom: 90rpx;
}

.content text {
  display: block;
  color: #9d9d9d;
  margin-top: 40rpx;
}

.bottom {
  border-radius: 80rpx;
  margin: 70rpx 50rpx;
  font-size: 35rpx;
}
</style>
