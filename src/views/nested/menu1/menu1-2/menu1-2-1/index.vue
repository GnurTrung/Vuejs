<template>
  <div class="app-container">
    <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px"
      @click="toggleCreateInformation"
    >
      Thêm Công Việc Mới
    </el-button>

    <el-dialog
      title="Thêm Công Việc Mới"
      :visible.sync="dialogFormVisible"
      @confirm="handleConfirm"
    >
      <el-form :model="form" @submit.stop.prevent="handleSubmit">
        <el-form-item label="Tên Công Việc" :label-width="formLabelWidth">
          <el-input v-model="form.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Nội Dung" :label-width="formLabelWidth">
          <el-input v-model="form.content" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Ngày Khởi Công" :label-width="formLabelWidth">
          <el-input v-model="form.date" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Người Phụ Trách" :label-width="formLabelWidth">
          <el-input v-model="form.boss" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Trạng Thái" :label-width="formLabelWidth">
          <el-checkbox v-model="form.check">Hoàn thành</el-checkbox>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="handleCancle">Hủy</el-button>
        <el-button type="primary" @click="handleSubmit">Xác Nhận</el-button>
      </span>
    </el-dialog>

    <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px"
      @click="toggleCreateFile"
    >
      Thêm Tài Liệu
    </el-button>
      <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px"
      @click="handleVatlieu"
    >
      Danh sách vật liệu
    </el-button>
          <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px"
      @click="handleCacben"
    >
      Danh sách các bên
    </el-button>

    <!--table-->
    <el-table border stripe :data="tableData" style="width: 100%">
      <el-table-column
        align="center"
        prop="name"
        label="Tên công việc"
        width="300"
      >
      </el-table-column>
      <el-table-column
        align="center"
        prop="content"
        label="Nội dung công việc"
        width="250"
      >
      </el-table-column>
      <el-table-column
        align="center"
        prop="date"
        sortable
        label="Ngày khởi công"
        width="200"
      >
      </el-table-column>
      <el-table-column
        align="center"
        prop="boss"
        label="Người phụ trách"
        width="250"
      >
      </el-table-column>
      <el-table-column
        align="center"
        prop="status"
        label="Trạng thái"
        width="200"
      >
        <template slot-scope="scope">
          <el-checkbox prop="check" v-model="scope.row.check"
            >Hoàn thành</el-checkbox
          >
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
          <el-button icon="el-icon-view" type="primary" size="mini"></el-button>
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
          name: "Công việc con 1",
          date: "21/12/2018",
          content: "Đào đất, Trộn vữa",
          boss: "Tùng Hồ",
          check: true,
          sign: "2/4",
          job: 3,
          assign: ["Công ty A"],
        },
        {
          name: "Công việc con 2",
          date: "19/2/2019",
          content: "Đào đất, Kiểm tra móng",
          boss: "Đào NC",
          check: true,
          sign: "3/4",
          job: 1,
          assign: ["Công ty A", "Công ty B"],
        },
        {
          name: "Công việc con 3",
          date: "1/1/2019",
          content: "Trộn vữa",
          boss: "Hảo HT",
          check: false,
          sign: "0/5",
          job: 5,
          assign: [],
        },
      ],
      form: {
        name: "",
        content: "",
        date: "",
        boss: "",
        check: "",
      },
      formType: "",
      dialogFormVisible: false,
      formLabelWidth: "120px",
    };
  },
  methods: {
    toggleCreateInformation() {
      this.dialogFormVisible = true;
      this.formType = "create";
    },
        handleVatlieu(){
      this.$router.push('/nested/menu1/menu1-3')
    },
        handleCacben(){
      this.$router.push('/nested/menu1/menu1-4')
    },
    handleEdit(data) {
      this.dialogFormVisible = true;
      this.formType = "edit";
      this.form = { ...data };
    },
    handleDelete(index) {
      this.tableData.splice(index, 1);
      this.$refs.form.resetFields();
    },
    handleSubmit() {
      const data = this.form;
      switch (this.formType) {
        case "create":
          this.tableData.push(data);
          break;
        case "edit":
          this.tableData.splice(this.tableData.indexOf(data) - 1, 1, data);
          break;
        default:
          break;
      }
      this.form = {
        name: "",
        unit: "",
        price: "",
        supplier: "",
      };
      this.dialogFormVisible = false;
    },
    handleCancle() {
      this.dialogFormVisible = false;
      this.form = {
        name: "",
        unit: "",
        price: "",
        supplier: "",
      };
    },
    handleClose(done) {
      this.$confirm("Are you sure to close this dialog?")
        .then((_) => {
          done();
        })
        .catch((_) => {});
    },
  },
};
</script>

<style scoped>
</style>
