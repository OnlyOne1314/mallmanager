<template>
<el-card class="box-card">
  <el-breadcrumb separator-class="el-icon-arrow-right">
    <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
    <el-breadcrumb-item>用户管理</el-breadcrumb-item>
    <el-breadcrumb-item>用户列表</el-breadcrumb-item>
  </el-breadcrumb>
  <el-row class="searchRow">
    <el-co>
      <el-input @clear="loadUser()" clearable placeholder="请输入内容" v-model="query" class="inputSearch">
        <el-button @click="searchUser()" slot="append" icon="el-icon-search"></el-button>
      </el-input>
      <el-button type="success" @click="showAddDia()">添加用户</el-button>
    </el-co>
  </el-row>
  <el-table :data="userlist" style="width: 100%">
    <el-table-column type="index" label="#" width="60">
    </el-table-column>
    <el-table-column prop="name" label="姓名" width="80">
    </el-table-column>
    <el-table-column prop="email" label="邮箱">
    </el-table-column>
    <el-table-column prop="phone" label="电话">
    </el-table-column>
    <el-table-column prop="date" label="创建日期">
      <template slot-scope="userlist">
        {{userlist.row.date | fmtdate}}
      </template>
    </el-table-column>
    <el-table-column label="用户状态">
      <template slot-scope="scope">
        <el-switch v-model="scope.row.mg_State" active-color="#13ce66" inactive-color="#ff4949">
        </el-switch>
      </template>
    </el-table-column>
    <el-table-column prop="address" label="操作">
      <template slot-scope="scope">
        <el-button size="mini" plain type="primary" icon="el-icon-edit" circle @click="showEditDia(scope.row)"></el-button>
        <el-button size="mini" plain type="success" icon="el-icon-check" circle @click="showCheckUser(scope.row)"></el-button>
        <el-button size="mini" plain type="danger" icon="el-icon-delete" circle @click="showDeleUser(scope.row.id)"></el-button>
      </template>
    </el-table-column>
  </el-table>
  <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="pagenum" :page-sizes="[2, 4, 6, 8]" :page-size="2" layout="total, sizes, prev, pager, next, jumper" :total="total">
  </el-pagination>
  <el-dialog title="添加用户" :visible.sync="dialogFormVisibleAdd">
    <el-form :model="form">
      <el-form-item label="用户名" label-width="100px">
        <el-input v-model="form.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="密 码" label-width="100px">
        <el-input v-model="form.password" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="邮 箱" label-width="100px">
        <el-input v-model="form.email" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="电 话" label-width="100px">
        <el-input v-model="form.phone" autocomplete="off"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogFormVisibleAdd = false">取 消</el-button>
      <el-button type="primary" @click="addUser()">确 定</el-button>
    </div>
  </el-dialog>
  <el-dialog title="编辑用户" :visible.sync="dialogFormVisibleEdit">
    <el-form :model="form">
      <el-form-item label="用户名" label-width="100px">
        <el-input disabled v-model="form.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="邮 箱" label-width="100px">
        <el-input v-model="form.email" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="电 话" label-width="100px">
        <el-input v-model="form.phone" autocomplete="off"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogFormVisibleEdit = false">取 消</el-button>
      <el-button type="primary" @click="editUser()">确 定</el-button>
    </div>
  </el-dialog>
  <el-dialog title="分配角色" :visible.sync="dialogFormVisibleCheck">
  <el-form :model="form">
    <el-form-item label="用户名" label-width="100px">
      <el-input v-model="form.name" autocomplete="off"></el-input>
    </el-form-item>
    <el-form-item label="角色" label-width="100px">
      <el-select v-model="form.region" placeholder="请选择">
        <el-option label="超级管理员" value="shanghai"></el-option>
        <el-option label="测试角色" value="beijing"></el-option>
      </el-select>
    </el-form-item>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisibleCheck = false">取 消</el-button>
    <el-button type="primary" @click="dialogFormVisibleCheck = false">确 定</el-button>
  </div>
</el-dialog>
</el-card>
</template>

<script>
export default {
  data () {
    return {
      query: '',
      userlist: [{
        date: '2016-05-02',
        email: '1234@qq',
        phone: '123456789',
        username: '王小虎',
        address: '',
        mg_State: true
      }],
      // 分页
      pagenum: 1,
      pagesize: 2,
      // 总数
      total: 1,
      dialogFormVisibleAdd: false,
      dialogFormVisibleEdit: false,
      dialogFormVisibleCheck: false,
      form: {
        username: '',
        password: '',
        email: '',
        phone: ''
      }
    }
  },
  created () {
    this.getUserList()
  },
  methods: {
    showCheckUser () {
      this.dialogFormVisibleCheck = true
    },
    // 编辑
    showEditDia () {
      this.form = this.userlist
      this.dialogFormVisibleEdit = true
    },
    // 删除
    showDeleUser (userId) {
      this.$confirm('删除用户?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async () => {
        // eslint-disable-next-line no-template-curly-in-string
        const res = await this.$http.delete('users/${userId}')
        console.log(res)
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(() => {
        this.$message({
          type: 'info',
          message: '已取消删除'
        })
      })
    },
    showAddDia () {
      this.dialogFormVisibleAdd = true
    },
    // 清空搜索框
    loadUser () {
      this.getUserList()
    },
    // 搜索
    searchUser () {
      this.getUserList()
    },
    // 分页
    handleSizeChange (val) {
      console.log(`每页 ${val} 条`)
      this.pagesize = val
      this.getUserList()
    },
    handleCurrentChange (val) {
      console.log(`当前页: ${val}`)
      this.pagenum = val
      this.getUserList()
    },
    async getUserList () {
      // 授权api,必须请求头中使用 Authorization 字段提供 token 令牌
      const AUTH_TOKEN = localStorage.getItem('token')
      this.$http.defaults.headers.common['Authorization'] = AUTH_TOKEN
      // 请求
      const res = await this.$http.get(`users?query=${this.query}&pagenum=${this.pagenum}&pagesize=${this.pagesize}`)
      console.log(res)
      // eslint-disable-next-line no-unused-vars
      const {
        meta: {
          status,
          msg
        },
        data: {
          users,
          total
        }
      } = res.data
      if (status === 200) {
        this.userlist = users
        this.total = total
        this.$message.success(msg)
      } else {
        this.$message.warning(msg)
      }
    }
  }
}
</script>

<style>
.box-card {
  height: 100%;
}

.inputSearch {
  width: 400px;
}

.searchRow {
  margin-top: 20px
}
</style>
