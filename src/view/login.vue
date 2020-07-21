<template>
  <div class="login-container">
    <el-form :model="loginForm" :rules="rules2" status-icon ref="ruleForm2" label-position="left" label-width="0px" class="demo-ruleForm login-page">
      <h3 class="title">系统登录</h3>

      <el-form-item prop="username">
        <el-input type="text" v-model="loginForm.username" auto-complete="off" placeholder="用户名"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input type="password" v-model="loginForm.password" auto-complete="off" placeholder="密码"></el-input>
      </el-form-item>
      <el-checkbox v-model="checked" class="rememberme">记住密码</el-checkbox>
      <el-form-item style="width:100%;">
        <el-button type="primary" style="width:100%;" @click="login" :loading="logining">登录</el-button>
      </el-form-item>
   </el-form>
  </div>
</template>

<script>
import { mapMutations } from 'vuex';
export default {
  data () {
    return {
      logining: false,
      loginForm: {
        username: '',
        password: ''
      },
      rules2: {
        username: [{required: true, message: 'please enter your account', trigger: 'blur'}],
        password: [{required: true, message: 'enter your password', trigger: 'blur'}]
      },
      checked: false
    };
  },

  methods: {
    ...mapMutations(['changeLogin']),
  login () {
    let _this = this;
    if (this.loginForm.username === '' || this.loginForm.password === '') {
      alert('账号或密码不能为空');
    } else {
      this.$axios({
        method: 'post',
        url: 'http://timemeetyou.com:8889/api/private/v1/login',
        data: _this.loginForm
      }).then(res => {
        console.log(res.data);
      if(res.data.data==null||res.data.data==''){
        alert(res.data.meta.msg)
      }else {
        _this.userToken = 'Bearer ' + res.data.data.token;
        // 将用户token保存到vuex中
        _this.changeLogin({ Authorization: _this.userToken });
        _this.$router.push('/home');
      }
    }).catch(error => {
        alert('账号或密码错误');
      console.log(error);
    });
    }
  }
}
};
</script>

<style scoped>
.login-container {
  width: 100%;
  height: 100%;
}
.login-page {
  -webkit-border-radius: 5px;
  border-radius: 5px;
  margin: 180px auto;
  width: 350px;
  padding: 35px 35px 15px;
  background: #fff;
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
}
label.el-checkbox.rememberme {
  margin: 0px 0px 15px;
  text-align: left;
}
</style>
