<template>
  <div class="login-container">
    <div class="login-box">
      <div class="avatar">
        <img src="../assets/logo.png" alt />
      </div>
      <!-- 表单 -->
      <el-form label-width="0px" class="login_form" :model="form" :rules="rules" ref="loginRef">
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input v-model="form.username" placeholder="请输入用户名"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input v-model="form.password" placeholder="请输入密码" type="password"></el-input>
        </el-form-item>
        <el-form-item class="btn">
          <el-button type="primary" @click="loginFrom">登录</el-button>
          <el-button type="info" @click="resetLogin">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      form: {
        username: "",
        password: ""
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名", trigger: "blur" },
          { min: 3, max: 10, message: "长度在 3 到 10 个字符", trigger: "blur" }
        ],
        password: [
            { required: true, message: "请输入密码", trigger: "blur" },
            { min: 6, max: 16, message: "长度在 6 到 10 个字符", trigger: "blur" }
        ]
      }
    };
  },
  methods: {
      resetLogin() {
          this.$refs.loginRef.resetFields()
      },
      loginFrom() {
        //   console.log(this.$http)
        this.$refs.loginRef.validate(async valid => {
       
            if(!valid) return;
            const { data: res} = await this.$http.post("login", this.form) 
            console.log(res)
            if(res.meta.status !=200) return this.$message.error('登陆失败')
            this.$message.success('登陆成功')
            window.sessionStorage.setItem("token",res.data.token)
            this.$router.push('/home')

        });
      }
  }
};
</script>
<style lang="less">
.login-container {
  background-color: #2b4b6b;
  height: 100%;
  .login-box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    .avatar {
      width: 130px;
      height: 130px;
      border: 1px solid #eee;
      box-shadow: 0 0 5px #ddd;
      border-radius: 50%;
      padding: 10px;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      background-color: #fff;
      img {
        width: 130px;
        height: 130px;
        border-radius: 50%;
        background-color: #eee;
      }
    }
  }
}
.btn {
  display: flex;
  justify-content: flex-end;
}
.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
</style>