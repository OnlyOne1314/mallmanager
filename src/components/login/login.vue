<template>
<div class="login-wrap">
    <el-form label-position="top" label-width="80px" :model="formdata" class="login-form">
        <h2>用户登录</h2>
        <el-form-item label="用户名">
            <el-input v-model="formdata.username"></el-input>
        </el-form-item>
        <el-form-item label="密码">
            <el-input v-model="formdata.password"></el-input>
        </el-form-item>
        <el-button type="primary" class="login-btn" @click.prevent="">登录</el-button>
    </el-form>
</div>
</template>

<script>
export default {
  data () {
    return {
      formdata: {
        username: '',
        password: ''
      }
    }
  },
  methods: {
    // 让异步操作的代码 看起来像同步代码
    async handLogin () {
      const res = await this.$http.post('login', this.formdata)
      const {
        // eslint-disable-next-line no-unused-vars
        data, meta: {msg, staus}
      } = res.data
      if (staus === 200) {
        localStorage.setItem('token', data.token)
        this.$router.push({name: 'home'})
        this.$message.success(msg)
      } else {
        this.$message.warning(msg)
      }
    }
    // handLogin () {
    //   this.$http.post('login', this.formdata).then(res => {
    //     const {
    //       // eslint-disable-next-line no-unused-vars
    //       data, meta: {msg, staus}
    //     } = res.data
    //     if (staus === 200) {
    //       this.$router.push({name: 'home'})
    //       this.$message.success(msg)
    //     } else {
    //       this.$message.warning(msg)
    //     }
    //   })
    // }
  }
}
</script>

<style>
.login-wrap {
    height: 100%;
    background-image: url(../../assets/denglu.jpg);
    background-repeat:no-repeat;
    background-size:100% 100%;
    -moz-background-size:100% 100%;
    display: flex;
    justify-content: center;
    align-items: center;
}
.login-wrap .login-form {
    width: 400px;background-color: #fff;
    border-radius: 5px;
    padding: 30px;
}
.login-wrap .login-btn {
    width: 100%;
}
</style>
