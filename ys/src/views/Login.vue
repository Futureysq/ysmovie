<template>
  <div class="register">
    <div class="logo-box">
      <div class="logo">
      </div>
    </div>
    <div class="list">
      <van-cell-group>
        <van-field
          v-model="user.value"
          type="text"
          label="用户名"
          placeholder="请输入用户名"
          :error-message="user.errorMessage"
          @input="validPa(user.value, 'validUsername', {key: 'user', errorMessage: '用户名支持4-8个(字母数字_)组合，必须字母开头'})"
        />
        <van-field
          v-model="passw1.value"
          label="密码"
          :type="passw1.type"
          placeholder="请输入密码"
          :error-message="passw1.errorMessage"
          :right-icon="passw1.icon"
          @click-right-icon="changeEye('passw1')"
          @input="validPa(passw1.value,'validPwd',{key: 'passw1',errorMessage: '密码支持6-16个(字母数字_)组合，必须含有大小写字母'})"
        />
      </van-cell-group>
      <van-button
        class="rgister-c-fz"
        block
        :loading="isLoading"
        @click="login"
        :disabled="!isValid"
        type="info"
        loading-text="登录中..."
        text="登录"
      />
      <div class="footer">
        <router-link class="login-text" to="/register">没有账号？注册</router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { validForm } from "../tool/validForm";

import { createNamespacedHelpers } from "vuex";

const { mapState, mapMutations } = createNamespacedHelpers("loginModule");

export default {
  name: "login",

  //计算属性
  computed: {
    ...mapState(["passw1", "isLoading", "isValid", "user"])
  },

  methods: {
    ...mapMutations(["changeEye"]),
    login() {
      let self = this;
      //注册
      this.$store.commit("loginModule/login", true);
      let randomTime = Math.random() * 5 * 1000;

      setTimeout(() => {
        //获取所有用户信息
        let users = localStorage.getItem("users");

        users = users ? JSON.parse(users) : [];

        if (users.length === 0) {
          this.$toast({
            duration: 2000,
            message: '该用户不存在'
          })
          this.$store.commit('loginModule/login', false);
          return;
        }

        //查找用户
        for (let i = 0; i < users.length; i++) {
          if (users[i].username == this.user.value) {
              if (users[i].pwd == this.passw1.value) {
                this.$toast({
                  duration: 2000,
                  message: '登录成功'
                })
                this.$store.commit('loginModule/login', false);
                // 跳转到首页
                // 记录用户登录状态
                let user = {
                  isLogin: true,
                  username: this.user.value
                };
                
                localStorage.setItem('user', JSON.stringify(user));

                return this.$router.push({name: 'home'});
              } else {
                this.$toast({
                  duration: 2000,
                  message: '用户名或者密码不正确'
                })
                this.$store.commit('loginModule/login', false);
                return;
              }
            }
          }

        this.$toast({
          duration: 2000,
          message: "该用户不存在"
        });

        this.$store.commit("loginModule/login", false);       
      },randomTime)
    },

    //验证用户名或者密码
    validPa(value, fnName, o) {
      let isValid = validForm[fnName](value);

      o.isValid = isValid;

      this.$store.commit("loginModule/validPa", o);

      this.$store.commit("loginModule/passForm");
    }
  }
};
</script>

<style lang="less" scoped>
.register{
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  // height: 100%;

  // background: url("../assets/images/p2574278284.jpg");
  // background-size: cover;
.logo-box {
  height: 200px;
  position: relative;
  // background: url("../assets/images/register_bg.png");
  background-size: cover;
  .logo {
    width: 100px;
    height: 100px;
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    margin: auto;
    background: url("../assets/images/96dda144ad345982baf7b0ed0af431adcbef844b.jpg");
    background-size: cover;
    // background-color: antiquewhite;
    border-radius: 50%;
  }
}
.list {
  padding: 10px;
}
.footer {
  margin-top: 20px;
  color: #ddd;
}
.rgister-c-fz {
  font-size: 18px;
}
.login-text {
  color: #666;
}  
}
</style>