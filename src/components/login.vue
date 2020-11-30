<template>
  <div class="login-cc">
    <div class="login-box">
      <div class="ava-box">
        <img src="../assets/logo.png" alt="logo" />
      </div>
      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login-form">
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input type="password" v-model="loginForm.password" prefix-icon="el-icon-lock"></el-input>
        </el-form-item>
        <el-form-item class="btns">
          <el-button type="primary" @click="refLoginForm()">登录</el-button>
          <el-button type="info" @click="restLoginFrom()">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        loginForm: {
          username: '',
          password: ''
        },
        loginFormRules: {
          username: [{
              required: true,
              message: '请输入登录名称',
              trigger: 'blur'
            },
            {
              min: 3,
              max: 10,
              message: '长度在 3 到 10 个字符',
              trigger: 'blur'
            }
          ],
          password: [{
              required: true,
              message: '请输入登录密码',
              trigger: 'blur'
            },
            {
              min: 5,
              max: 15,
              message: '长度在 5 到 15 个字符',
              trigger: 'blur'
            }
          ]
        }
      }
    },
    methods: {
      restLoginFrom() {
        this.$refs.loginFormRef.resetFields()
      },
      refLoginForm() {
        this.$refs.loginFormRef.validate(async valid => {
          if (!valid) return;
          const {
            data: res
          } = await this.$http.post("/user/login",
            "username=" + this.loginForm.username + "&password=" + this.loginForm.password)
          if (res.code !== 200) return this.$message.error(res.msg)
          this.$message.success(res.msg)
          window.localStorage.setItem("token", res.data)
          this.$router.push("/home")
        })
      }
    }
  }
</script>

<style lang="less" scoped>
  .login-cc {
    height: 100%;
    background-color: #2b4b6b;
  }

  .login-box {
    width: 450px;
    height: 300px;
    background-color: #ffffff;
    border-radius: 5px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);

    .ava-box {
      height: 130px;
      width: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 10px;
      box-shadow: 0 0 10px #ddd;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -40%);
      background-color: #fff;

      img {
        height: 100%;
        width: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
  }

  .btns {
    display: flex;
    justify-content: flex-end;
  }

  .login-form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 30px;
    box-sizing: border-box;
  }
</style>
