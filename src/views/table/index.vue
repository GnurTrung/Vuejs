<template>
  <div class="app-container">
    <title>Quản lý danh sách dự án</title>
    <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px;"
      @click="toggleCreateProject"
    >
      Thêm dự án
    </el-button>
    <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px; margin-left: 20px;"
      @click="toggleCreateDocument"
    >
      Thêm tài liệu
    </el-button>

    <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px; margin-left: 20px;"
      @click="handleSupplierList"
    >
      Danh sách bên liên quan
    </el-button>
    <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px; margin-left: 20px;"
      @click="handleMaterialList"
    >
      Danh sách nguyên vật liệu
    </el-button>

    <el-dialog
      title="Thông tin dự án"
      :visible.sync="dialogFormVisible"
      :before-close="handleClose"
    >
      <el-form ref="form" :model="form" label-width="120px" label-position="top">
        <el-form-item label="Tên dự án" prop="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="Ngày bắt đầu" prop="createAt">
          <el-input v-model="form.createAt"></el-input>
        </el-form-item>
        <el-form-item label="Người quản lý" prop="manager">
          <el-input v-model="form.manager"></el-input>
        </el-form-item>
        <el-form-item label="Trạng thái" prop="status">
          <el-checkbox v-model="form.status">Hoàn thành</el-checkbox>
        </el-form-item>
        <el-form-item>
          <el-button @click="handleCancle">Hủy</el-button>
          <el-button type="primary" @click="handleSubmit">Xác nhận</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <el-table
      border
      stripe
      :data="tableData"
      style="width:100%"
    >
      <el-table-column label="Tên dự án" prop="name" align="left" width="200">
        <template slot-scope="scope">
          <div style="display: flex; justify-content: flex-start">
            <div v-if="scope.row.type === 'folder'">
              <i class="el-icon-folder"></i>
            </div>
            <div v-else-if="scope.row.type === 'file'">
              <i class="el-icon-document"></i>
            </div>
            <div style="margin-left: 5px">{{ scope.row.name }}</div>
          </div>
        </template>
      </el-table-column>
      <el-table-column label="Ngày bắt đầu" prop="createAt" align="center" width="200">
        <template slot-scope="scope">
          <span>{{ scope.row.createAt }}</span>
        </template>
      </el-table-column>
      <el-table-column label="Người quản lý" prop="manager" align="center" width="200">
        <template slot-scope="scope">
          <span>{{ scope.row.manager }}</span>
        </template>
      </el-table-column>
      <el-table-column label="Trạng thái" prop="status" align="center" width="200">
        <template slot-scope="scope">
          <el-checkbox v-model="scope.row.status">Hoàn thành</el-checkbox>
        </template>
      </el-table-column>

      <!--custom-->
      <el-table-column
        label="Thao tác"
        align="center"
        width="180"
        fixed="right"
      >
        <template slot-scope="scope">
          <el-button
            icon="el-icon-view"
            type="primary"
            size="mini"
            @click="handleView"
          >

          </el-button>
          <el-button
            icon="el-icon-edit"
            type="primary"
            size="mini"
            @click="handleEdit(scope.row, scope.$index)"
          ></el-button>
          <el-popconfirm
            style="margin-left: 5px"
            title="Bạn có chắc chắn xóa ?"
            cancel-button-text="Hủy"
            confirm-button-text="Đồng ý"
            @onConfirm="handleDelete(scope.$index)"
          >
            <el-button
              slot="reference"
              icon="el-icon-delete"
              type="danger"
              size="mini"
            ></el-button>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tableData: [
        {
          projectID: 0,
          name: 'Dự án 1',
          createAt: '22/8/2021',
          manager: 'Nguyễn Văn A',
          status: false,
          type: 'folder'
        }
      ],
      form: {
        projectID: '',
        type: '',
        name: '',
        createAt: '',
        manager: '',
        status: null
      },
      formType: '',
      dialogFormVisible: false,
      formLabelWidth: '120px'
    }
  },
  methods: {
    toggleCreateProject() {
      this.dialogFormVisible = true
      this.formType = 'create'
      this.form.type = 'folder'
    },
    toggleCreateDocument() {
      this.dialogFormVisible = true
      this.formType = 'create'
      this.form.type = 'file'
    },
    handleSupplierList() {
      const url = '/manage/' + this.tableData.name + '/phase'
      this.$router.push(url)
    },
    handleMaterialList() {
      this.$router.push('/manage/material')
    },
    handleView() {
      // /project-manage/project/:id/phase
      // const url = '/project-manage/project/' + this.tableData.projectID.toString + '/phase'
      // this.$router.push({
      //   path: url,
      //   name: 'Project 1'
      // })
     this.$route.push({ path: '/project-manage/project/:id/phase', params: { }})
    },
    handleEdit(data, index) {
      this.dialogFormVisible = true
      this.formType = 'edit'
      this.form = { index, ...data }
    },
    handleDelete(index) {
      this.tableData.splice(index, 1)
      this.$refs.form.resetFields()
    },
    handleSubmit() {
      const data = this.form
      const index = this.form.index
      switch (this.formType) {
        case 'create':
          this.tableData.push(data)
          break
        case 'edit':
          this.tableData.splice(index, 1, data)
          break
        default:
          break
      }
      this.form = {
        type: '',
        name: '',
        createAt: '',
        manager: '',
        status: null
      }
      this.dialogFormVisible = false
    },
    handleCancle() {
      this.dialogFormVisible = false
    },
    handleClose(done) {
      this.$confirm('Are you sure to close this dialog?')
        .then(_ => {
          done()
          this.form = {
            type: '',
            name: '',
            createAt: '',
            manager: '',
            status: null
          }
        })
        .catch(_ => {})
    }
  }
}
</script>

<style scoped>

</style>
