<template>
  <div class="app-container">
    <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px"
      @click="toggleCreateProject"
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
        <!-- <el-form-item label="Loại file" prop="type">
          <el-radio v-model="form.type" label="folder">Folder</el-radio>
          <el-radio v-model="form.type" label="file">Document</el-radio>
        </el-form-item> -->
        <el-form-item label="Ngày Khởi Tạo" :label-width="formLabelWidth">
          <el-input v-model="form.date" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Người Quản Lý" :label-width="formLabelWidth">
          <el-input v-model="form.boss" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Đã Ký" :label-width="formLabelWidth">
          <el-input v-model="form.sign" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Công Việc Con" :label-width="formLabelWidth">
          <el-input v-model="form.job" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="Trạng Thái" :label-width="formLabelWidth">
          <el-checkbox v-model="form.check">Hoàn thành</el-checkbox>
        </el-form-item>
        <el-form-item label="Các bên" :label-width="formLabelWidth">
          <el-select v-model="form.assign" multiple placeholder="Select">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            >
            </el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="handleCancle">Hủy</el-button>
        <el-button type="primary" @click="handleSubmit">Xác Nhận</el-button>
      </span>
    </el-dialog>

  <!-- btn Upload -->
  <el-button
      type="primary"
      size="medium"
      style="margin-bottom: 20px"
      @click="toggleCreateDocument"
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

    <el-table border stripe :data="tableData" style="width: 100%">
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
      <el-table-column
        align="center"
        prop="date"
        sortable
        label="Ngày khởi tạo"
        width="180"
      >
      </el-table-column>
      <el-table-column
        align="center"
        prop="boss"
        label="Người quản lý"
        width="150"
      >
      </el-table-column>
      <el-table-column
        align="center"
        prop="sign"
        sortable
        label="Đã ký"
        width="100"
      >
      </el-table-column>
      <el-table-column
        align="center"
        prop="job"
        sortable
        label="Công việc con"
        width="150"
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
      <el-table-column align="center" prop="co" label="Gán" width="250">
        <template slot-scope="scope">
          <el-select v-model="scope.row.assign" multiple placeholder="Select">
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            >
            </el-option>
          </el-select>
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
          <el-button icon="el-icon-view" type="primary" size="mini" @click="handleView()"></el-button>
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
      options: [
        {
          value: "Option1",
          label: "Công ty A",
        },
        {
          value: "Option2",
          label: "Công ty B",
        },
        {
          value: "Option3",
          label: "Công ty C",
        },
        {
          value: "Option4",
          label: "Công ty D",
        },
        {
          value: "Option5",
          label: "Công ty E",
        },
      ],
      value1: [],
      tableData: [
        {
          type: 'folder',
          name: "Công việc 1",
          date: "21/12/2018",
          content: "Đào đất, Trộn vữa",
          boss: "Tùng Hồ",
          check: true,
          sign: "2/4",
          job: 3,
          assign: ["Công ty A"],
        },
        {
          type: 'folder',
          name: "Công việc 2",
          date: "19/2/2019",
          content: "Đào đất, Kiểm tra móng",
          boss: "Đào NC",
          check: true,
          sign: "3/4",
          job: 1,
          assign: ["Công ty A", "Công ty B"],
        },
        {
          type: 'file',
          name: "Tài liệu 1",
          date: "1/1/2019",
          check: false,
          sign: "0/5",
          assign: [],
        },
      ],
      form: {
        type: '',
        name: "",
        date: "",
        boss: "",
        sign: "",
        job: "",
        check: "",
      },
      
      formType: "",
      dialogFormVisible: false,
      formLabelWidth: "120px",
    };
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
    handleVatlieu(){
      this.$router.push('/form/index')
    },
        handleCacben(){
      this.$router.push('/form2/index')
    },
    handleView(){
      this.$router.push('/nested/menu1/menu1-2/menu1-2-1')
    },
    handleEdit(data, index) {
      this.dialogFormVisible = true;
      this.formType = 'edit';
      this.form = {  index, ...data };
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
