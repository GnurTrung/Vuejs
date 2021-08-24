<template>
  <div class="app-container">
    <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px;"
      @click="toggleCreateInformation"
    >
      Thêm nguyên vật liệu
    </el-button>

    <el-dialog
      title="Thông tin nguyên vật"
      :visible.sync="dialogFormVisible"
      :before-close="handleClose"
    >
      <el-form :model="form" ref="form" label-width="120px" label-position="top">
        <el-form-item label="Tên vật liệu" prop="name">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="Loại file" prop="type">
          <el-radio v-model="form.type" label="folder">Folder</el-radio>
          <el-radio v-model="form.type" label="file">Document</el-radio>
        </el-form-item>
        <el-form-item label="Đơn vị" prop="unit">
          <el-input v-model="form.unit"></el-input>
        </el-form-item>
        <el-form-item label="Đơn giá" prop="price">
          <el-input v-model="form.price"></el-input>
        </el-form-item>
        <el-form-item label="Đơn vị cung cấp" prop="supplier">
          <el-input v-model="form.supplier"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="handleCancle">Hủy</el-button>
          <el-button type="primary" @click="handleSubmit">Xác nhận</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>

    <!--table-->
    <el-table
      border
      stripe
      :data="tableData"
      style="width:100%"
    >
      <el-table-column label="Tên vật liệu" prop="name" align="center" width="200">
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
      <el-table-column label="Đơn vị" prop="unit" align="center" width="220">
        <template slot-scope="scope">
          <span>{{ scope.row.unit }}</span>
        </template>
      </el-table-column>
      <el-table-column label="Đơn giá" prop="phone" align="center" width="250">
        <template slot-scope="scope">
          <span>{{ scope.row.price }}</span>
        </template>
      </el-table-column>
      <el-table-column label="Đơn vị cung cấp" prop="detail" align="center" width="300">
        <template slot-scope="scope">
          <span>{{ scope.row.supplier }}</span>
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
            v-if="scope.row.type == 'folder'"
          ></el-button>
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
          name: 'Gạch',
          unit: 'm3',
          price: '1 triệu/m3',
          supplier: 'Công ty A'
        }
      ],
      form: {
        type: '',
        name: '',
        unit: '',
        price: '',
        supplier: ''
      },
      formType: '',
      dialogFormVisible: false,
      formLabelWidth: '120px'
    }
  },
  methods: {
    toggleCreateInformation() {
      this.dialogFormVisible = true
      this.formType = 'create'
    },
    handleEdit(data) {
      this.dialogFormVisible = true
      this.formType = 'edit'
      this.form = { ...data }
    },
    handleDelete(index) {
      this.tableData.splice(index, 1)
      this.$refs.form.resetFields()
    },
    handleSubmit() {
      const data = this.form
      switch (this.formType) {
        case 'create':
          this.tableData.push(data)
          break
        case 'edit':
          this.tableData.splice(this.tableData.indexOf(data) - 1, 1, data)
          break
        default:
          break
      }
      this.form = {
        type: '',
        name: '',
        unit: '',
        price: '',
        supplier: ''
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
            unit: '',
            price: '',
            supplier: ''
          }
        })
        .catch(_ => {})
    }
  }
}
</script>

<style scoped>

</style>
