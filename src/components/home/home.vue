<template>
<el-container class="container">
    <el-header class="header">
        <el-row>
            <el-col :span="4">
                <div class="grid-content bg-purple">
                    <img src="../../assets/logo.png" alt="无法显示图片">
                </div>
            </el-col>
            <el-col :span="18" class="middle">
                <h3>后台管理系统</h3>
            </el-col>
            <el-col :span="2">
                <div class="grid-content bg-purple">
                    <el-avatar icon="el-icon-user-solid" class="inout" @click="handnout"></el-avatar>
                </div>
            </el-col>
        </el-row>
    </el-header>
    <el-container>
        <el-aside width="200px" class="aside">
            <el-menu :unique-opened="true" :router="true">
                <el-submenu index="1">
                    <template slot="title">
                        <i class="el-icon-location"></i>
                        <span>用户管理</span>
                    </template>
                    <el-menu-item index="users">
                        <i class="el-icon-user-solid"></i>
                        <span>用户列表</span>
                    </el-menu-item>
                </el-submenu>
                <el-submenu index="2">
                    <template slot="title">
                        <i class="el-icon-s-tools"></i>
                        <span>权限管理</span>
                    </template>
                    <el-menu-item index="2-1">
                        <i class="el-icon-user"></i>
                        <span>角色列表</span>
                    </el-menu-item>
                    <el-menu-item index="2-2">
                        <i class="el-icon-location"></i>
                        <span>权限列表</span>
                    </el-menu-item>
                </el-submenu>
                <el-submenu index="3">
                    <template slot="title">
                        <i class="el-icon-picture-outline-round"></i>
                        <span>数据统计</span>
                    </template>
                    <el-menu-item index="3-1">
                        <i class="el-icon-location"></i>
                        <span>数据报表</span>
                    </el-menu-item>
                </el-submenu>
            </el-menu>
        </el-aside>
        <el-main class="main">
          <router-view></router-view>
        </el-main>
    </el-container>
</el-container>
</template>

<script>
export default {
  // 进入首页的权限验证
  // beforeCreate () {
  //   const token = localStorage.getItem('token')
  //   if (!token) {
  //     this.$router.push({name: 'login'})
  //   }
  // }
  // 退出登录
  methods: {
    // handnout () {
    // // 清除
    //   localStorage.clear()
    //   this.$message.success('退出成功')
    //   this.$router.push({name: 'login'})
    // }
  }
}
</script>

<style>
.container {
    height: 100%;
}

.header {
    background-color: #b3c0d1;
}

.aside {
    background-color: #d3dce6;
}

.main {
    background-color: #e9eef3;
}

.middle {
    text-align: center;
}

.inout {
    margin-top: 10px;
}
</style>
