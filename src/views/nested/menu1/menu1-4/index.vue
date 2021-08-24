<template>
  <div class="app-container">
    <!--Button Add-->
    <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px"
      @click="toggleCreateInfomation"
    >
      <span>Thêm bên liên quan</span>
    </el-button>
    <!--Form-->
    <el-dialog
      title="Thông tin các bên liên quan"
      :visible.sync="dialogFormVisible"
      :before-close="handleClose"
    >
      <el-form ref="relationForm" :model="relationForm" label-position="top">
        <el-form-item label="Tên" :label-width="formLabelWidth">
          <el-input v-model="relationForm.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Chức năng" :label-width="formLabelWidth">
          <el-input v-model="relationForm.feature" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Số điện thoại" :label-width="formLabelWidth">
          <el-input v-model="relationForm.phone" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Chi tiết" :label-width="formLabelWidth">
          <el-input v-model="relationForm.detail" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Địa chỉ" :label-width="formLabelWidth">
          <el-input v-model="relationForm.address" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click="dialogFormVisible = false">Hủy</el-button>
          <el-button
            type="primary"
            @click="handleSubmit()"
          >Xác nhận</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
    <!--Relation List Table-->
    <el-table
      border
      stripe
      :data="tableData"
      style="width:100%"
    >
      <el-table-column label="Tên" prop="name" align="center" width="220">
        <template slot-scope="scope">
          <span>{{ scope.row.name }}</span>
        </template>
      </el-table-column>
      <el-table-column label="Chức năng" prop="feature" align="center" width="220">
        <template slot-scope="scope">
          <span>{{ scope.row.feature }}</span>
        </template>
      </el-table-column>
      <el-table-column label="Số điện thoại" prop="phone" align="center" width="180">
        <template slot-scope="scope">
          <span>{{ scope.row.phone }}</span>
        </template>
      </el-table-column>
      <el-table-column label="Chi tiết" prop="detail" align="center" width="220">
        <template slot-scope="scope">
          <span>{{ scope.row.detail }}</span>
        </template>
      </el-table-column>
      <el-table-column label="Địa chỉ" prop="address" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.address }}</span>
        </template>
      </el-table-column>
      <!--custom-->
      <el-table-column
        label="Thao tác"
        align="center"
        width="180"
      >
        <template slot-scope="scope">
          <el-button
            icon="el-icon-view"
            type="primary"
            size="mini"
          ></el-button>
          <el-button
            icon="el-icon-edit"
            type="primary"
            size="mini"
            @click="handleEdit(scope.row)"
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
          name: 'Công ty A',
          feature: 'cung cấp',
          phone: '0947410528',
          detail: 'cát vàng',
          address: 'Thái Bình'
        },
                {
          name: 'Công ty B',
          feature: 'cung cấp',
          phone: '1234567989',
          detail: 'xi măng',
          address: 'Hà Nội'
        }
      ],
      relationForm: {
        name: '',
        feature: '',
        phone: '',
        detail: '',
        address: ''
      },
      dialogFormVisible: false,
      formLabelWidth: '120px'
    }
  },
  methods: {
    toggleCreateInfomation() {
      this.dialogFormVisible = true
    },
    handleSubmit() {
      this.tableData.push(this.relationForm)
      this.dialogFormVisible = false
      this.$refs.relationForm.refreshData();
    },
    handleEdit() {
      this.dialogFormVisible = true
    },
    handleDelete(index) {
      this.tableData.splice(index, 1)
    },
    handleClose(done) {
      this.$confirm('Are you sure to close this dialog?')
        .then(_ => {
          done()
        })
        .catch(_ => {})
    }
  }
}
</script>

<style scoped>

</style>
