<template>
  <div class="login">
    <el-form ref="form" :model="form" status-icon :rules="rules" label-width="80px">
      <img src="../assets/5.jpg" alt="">
      <el-form-item label="用户名" prop="username">
        <el-input v-model="form.username" placeholder="请输入用户名"></el-input>
      </el-form-item>

      <el-form-item label="密码" prop="password">
        <el-input v-model="form.password" placeholder="请输入密码" type="password"></el-input>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" @click="login">登录</el-button>
        <el-button  @click="reset">重置</el-button>
      </el-form-item>
  </el-form>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      form: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          // 必填项
          { required: true, message: '用户名不能为空', trigger: 'change' },
          // 校验长度
          { min: 3, max: 9, message: '长度在 3 到 9 个字符', trigger: 'change' }
        ],
        password: [
          { required: true, message: '密码不能为空', trigger: 'change' },
          // 校验长度
          {
            min: 6,
            max: 12,
            message: '长度在 6 到 12 个字符',
            trigger: 'change'
          }
        ]
      }
    }
  },
  methods: {
    reset() {
      this.$refs.form.resetFields()
    },
    login() {
      this.$refs.form.validate(valid => {
        if (valid) {
          // console.log('校验通过了')
          axios({
            url: 'http://localhost:8888/api/private/v1/login',
            method: 'post',
            data: this.form
          }).then(res => {
            if (res.data.meta.status === 200) {
              // 登录成功
              this.$message.success('登录成功')
              // 存储token
              localStorage.setItem('token', res.data.data.token)
              // console.log(res.data)
              // 跳转到Home组件
              this.$router.push('/home')
            } else {
              // 登录失败
              this.$message.error(res.data.meta.msg)
            }
          })
        } else {
          console.log('没通过')
          return false
        }
      })
    }
  }
}
</script>

<style lang="less">
.login {
  height: 100%;
  background-color: #2d434c;
  overflow: hidden;

  .el-form {
    width: 400px;
    margin: 200px auto;
    background-color: #fff;
    padding: 75px 40px 15px;
    border-radius: 20px;
    position: relative;

    img {
      position: absolute;
      width: 100px;
      height: 100px;
      left: 50%;
      transform: translateX(-50%);
      top: -80px;
      border-radius: 50%;
      border: 10px solid #fff;
    }
  }

  .el-button + .el-button {
    margin-left: 100px;
  }
}
</style>
