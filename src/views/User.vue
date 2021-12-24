<template>
  <div style="padding: 10px">
    <!--功能区域-->
    <div style="margin: 10px 0">
      <el-button type="primary" @click="add">新增</el-button>
      <el-button type="primary">导入</el-button>
      <el-button type="primary">导出</el-button>
    </div>
    <!--搜索区域-->
    <div style="margin: 10px 0">
      <el-input v-model="search" placeholder="请输入关键字" style="width: 20%"/>
      <el-button type="primary" style="margin-left: 5px" @click="load">查询</el-button>
    </div>
    <el-table :data="tableData" v-loading="loading" style="width: 100%" border stripe>
      <el-table-column prop="id" label="id" sortable />
      <el-table-column prop="name" label="姓名"/>
      <el-table-column prop="password" label="密码" />
      <el-table-column prop="age" label="年龄" />
      <el-table-column
              label="操作">
        <template #default="scope">
          <el-button size="mini" @click="handleEdit(scope.row)">编辑</el-button>
          <el-popconfirm title="确认删除吗?" @confirm="handleDelete(scope.row.id)">
            <template #reference>
              <el-button size="mini" type="danger">删除</el-button>
            </template>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog v-model="dialogVisible" title="提示" width="30%">
      <el-form :model="form" label-width="120px">
        <el-form-item label="用户名">
          <el-input v-model="form.name" style="width: 80%;"></el-input>
        </el-form-item>
        <el-form-item label="密码">
          <el-input v-model="form.password" style="width: 80%;"></el-input>
        </el-form-item>
        <el-form-item label="年龄">
          <el-input v-model="form.age" style="width: 80%;"></el-input>
        </el-form-item>
      </el-form>
      <template #footer>
        <span class="dialog-footer">
          <el-button @click="dialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="save">确 定</el-button>
        </span>
      </template>
    </el-dialog>

    <div style="margin: 10px 0;">
      <el-pagination
              v-model:currentPage="currentPage"
              :page-sizes="[5, 10, 20]"
              :page-size="pageSize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="total"
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
      >
      </el-pagination>
    </div>
  </div>
</template>

<script>
  import request from "../utils/request";

  export default {
    name: "User",
    data() {
      return {
        search: '',
        currentPage: 1,
        pageSize: 10,
        total: 10,
        tableData: [],
        loading: false,
        dialogVisible: false,
        form:{}
      }
    },
    created() {
      this.load()
    },
    methods: {
      handleEdit(row) {
        this.form = JSON.parse(JSON.stringify(row))
        this.dialogVisible = true
      },
      add() {
        this.dialogVisible = true
        this.form = {}
      },
      save() {
        if(this.form.id) { //存在意味着是更新操作
          request.post("/student/saveStudent", this.form).then(res =>{
            if (res.code == '0') {
              this.$message({
                type: "success",
                message: "修改成功"
              })
            }
            else {
              this.$message({
                type: "error",
                message: res.msg
              })
            }
          })
        } else {
          request.post("/student/saveStudent", this.form).then(res =>{
            if (res.code == '0') {
              this.$message({
                type: "success",
                message: "新增成功"
              })
            }
            else {
              this.$message({
                type: "error",
                message: res.msg
              })
            }
          })
        }
        this.load()
        this.dialogVisible = false
      },
      handleSizeChange(pageSize) {
        this.pageSize = pageSize
        this.load()
      },
      handleCurrentChange(pageNum) {
        this.currentPage = pageNum
        this.load()
      },
      handleDelete(id) {
        request.delete("/student/" + id).then(res => {
          if (res.code == '0') {
            this.$message({
              type: "success",
              message: "删除成功"
            })
          }
          else {
            this.$message({
              type: "error",
              message: res.msg
            })
          }
        })
        this.load()
      },
      load() {
        this.loading = true
        request.get("/student/getStudentsByPageAndSearch", {
          params: {
            pageNum: this.currentPage,
            pageSize: this.pageSize,
            search: this.search
          }
        }).then(res => {
          this.loading = false
          this.tableData = res.data.records
          this.total = res.data.total
        })
      }
    }
  }
</script>
