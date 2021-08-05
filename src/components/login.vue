<template>
  <div class="login-container">
    <h2 class="login-title">智能云联综合管理系统</h2>
    <a-form ref="form" :model="form" class="login-form">
      <h2 class="title">用户登录</h2>
      <a-form-item>
        <a-input class="inputBox" v-model="form.username">
          <img :src="user" slot="prefix" alt="">
        </a-input>
      </a-form-item>
      <a-form-item>
        <a-input-password class="inputBox" v-model="form.password">
          <img :src="mima" slot="prefix" alt="">
        </a-input-password>
      </a-form-item>

      <a-form-item>
        <a-button class="submit" type="primary" @click="onSubmit">登录</a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script>
import user from "../assets/user.png"
import mima from "../assets/密码.png"
export default {
  data() {
    return {
      user:user,
      mima:mima,
      form: {
        username:"admin",
        password:"123456"
      }
    }
  },
  methods: {
     onSubmit()  {
      console.log('登陆成功！')
    },
  },
}
</script>
<style>
.login-form {
  position: relative;
  width: 1063px;
  height: 604px;
  background: url("../assets/pic.png");
  float: left;
  left: 350px;
  top: -200px;
  background-size: 80%;
  background-repeat: no-repeat;
  padding: 40px 110px;
}

/* 背景 */
.login-container {
  position:fixed;
  top: 0;
  left: 0;
  width:100%;
  height:100%;
  min-width: 1000px;
  z-index:-10;
  zoom: 1;
  background-color: #fff;
  background-repeat: no-repeat;
  background-size: cover;
  -webkit-background-size: cover;
  -o-background-size: cover;
  background-position: center 0;
  background: url("../assets/bg.png");
}

/* Log */
.login-title {
  letter-spacing:10px;
  position: relative;
  top:-100px;
  color: #fff;
  text-align: center;
  margin: 150px 0;
  font-size: 30px;
  font-family: Microsoft Yahei;
}
/* 登陆按钮 */
.submit{
  width: 355px;
  height:36px;
  left: 95px;
  font-size: 17px;
}
/* 用户登陆标题 */
.title{
  left: 100px;
  top:30px;
  position: relative;
  margin-bottom: 50px;
  color: #fff;
  font-weight: 700;
  font-size: 24px;
  font-family: Microsoft Yahei;
}
/* 输入框 */
.inputBox{
  position: relative;
  height: 45px;
}
/* 输入框内左边距50px */
.ant-input-affix-wrapper .ant-input:not(:first-child) {
    padding-left: 50px;
    width: 355px;
    height: 36px;
    
}
.inputBox{
  left: 450px;
}
</style>