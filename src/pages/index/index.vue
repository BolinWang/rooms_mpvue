<template>
  <div class="container">
    <div class="userinfo" @click="bindViewTap">
      <img class="userinfo-avatar" v-if="userInfo.avatarUrl" :src="userInfo.avatarUrl" background-size="cover" />
      <div class="userinfo-nickname">
        <card :text="userInfo.nickName"></card>
      </div>
    </div>

    <div class="usermotto">
      <div class="user-motto">
        <card :text="motto"></card>
      </div>
    </div>

    <form class="form-container">
      <input type="text" class="form-control" v-model="motto" placeholder="v-model" />
      <input type="text" class="form-control" v-model.lazy="motto" placeholder="v-model.lazy" />
    </form>
    <a href="/pages/counter/main" class="counter">去往Vuex示例页面</a>
    <van-popup :show="showGetUserInfo" custom-class="popup_model">
      <div class="fht_model">
        <div class="fht_model_container">
          <div class="fht_model_title">提示</div>
          <div class="fht_model_content">获取用户授权</div>
          <div class="fht_model_footer">
            <van-button class="btn_footer" plain @click="showGetUserInfo = false">取消</van-button>
            <van-button class="btn_footer" type="primary" plain open-type="getUserInfo" @click="getUserInfo">授权</van-button>
          </div>
        </div>
      </div>
    </van-popup>
  </div>
</template>

<script>
import card from '@/components/card'
export default {
  data () {
    return {
      motto: 'Hello World',
      showGetUserInfo: false,
      userInfo: {}
    }
  },

  components: {
    card
  },

  methods: {
    bindViewTap () {
      const url = '../logs/main'
      wx.navigateTo({ url })
    },
    getUserInfo () {
      // 调用登录接口
      wx.login({
        success: () => {
          wx.getUserInfo({
            success: (res) => {
              this.userInfo = res.userInfo
            }
          })
        },
        complete: () => {
          this.showGetUserInfo = false
        }
      })
    }
  },

  created () {
    // 调用应用实例的方法获取全局数据
    this.showGetUserInfo = true
  }
}
</script>

<style lang="scss">
.popup_model {
  width: 80%;
}
</style>
<style scoped>
.userinfo {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.userinfo-avatar {
  width: 128rpx;
  height: 128rpx;
  margin: 20rpx;
  border-radius: 50%;
}

.userinfo-nickname {
  color: #aaa;
}

.usermotto {
  margin-top: 150px;
}

.form-control {
  display: block;
  padding: 0 12px;
  margin-bottom: 5px;
  border: 1px solid #ccc;
}

.counter {
  display: inline-block;
  margin: 10px auto;
  padding: 5px 10px;
  color: blue;
  border: 1px solid blue;
}
</style>
