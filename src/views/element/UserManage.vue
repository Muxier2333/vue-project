<template>
  <div class="user-manage">
    <!-- 搜索操作行 -->
    <div class="toolbar">
      <span class="toolbar-label">用户名</span>
      <el-input
        v-model="searchName"
        placeholder="请输入用户名"
        class="toolbar-input"
        clearable
      ></el-input>
      <el-button type="primary" @click="handleSearch">查询</el-button>
      <el-button type="success" @click="handleAdd">添加用户</el-button>
    </div>

    <!-- 用户表格 -->
    <el-table :data="pagedData" class="user-table" border>
      <el-table-column prop="date" label="日期" width="260">
        <template #default="{ row }">
          <span class="date-cell">
            <el-icon class="date-icon"><Clock /></el-icon>
            {{ row.date }}
          </span>
        </template>
      </el-table-column>
      <el-table-column prop="name" label="姓名" width="200">
        <template #default="{ row }">
          <el-tag class="name-tag">{{ row.name }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="220">
        <template #default="{ row }">
          <el-button @click="handleEdit(row)">编辑</el-button>
          <el-button type="danger" @click="handleDelete(row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>

    <!-- 分页 -->
    <el-pagination
      class="pagination"
      v-model:current-page="currentPage"
      :page-size="pageSize"
      :total="filteredData.length"
      layout="prev, pager, next, jumper"
      background
    ></el-pagination>

    <!-- 添加/编辑弹窗 -->
    <el-dialog
      v-model="dialogVisible"
      :title="dialogMode === 'add' ? '添加用户' : '编辑用户'"
      width="400px"
    >
      <el-form label-width="70px">
        <el-form-item label="用户名">
          <el-input v-model="form.name" placeholder="请输入用户名"></el-input>
        </el-form-item>
      </el-form>
      <template #footer>
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="handleDialogConfirm">确定</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script>
import { Clock } from "@element-plus/icons-vue";
import { ElMessageBox, ElMessage } from "element-plus";

export default {
  components: { Clock },
  data() {
    return {
      searchName: "",
      currentPage: 1,
      pageSize: 4,
      dialogVisible: false,
      dialogMode: "add",
      form: { id: null, name: "" },
      userList: [
        { id: 1, date: "2016-05-02", name: "王小虎" },
        { id: 2, date: "2016-05-04", name: "王小虎" },
        { id: 3, date: "2016-05-01", name: "王小虎" },
        { id: 4, date: "2016-05-03", name: "王小虎" },
        { id: 5, date: "2016-05-05", name: "李小龙" },
        { id: 6, date: "2016-05-06", name: "张小丽" },
        { id: 7, date: "2016-05-07", name: "王小虎" },
      ],
    };
  },
  computed: {
    filteredData() {
      if (!this.searchName) return this.userList;
      return this.userList.filter((u) => u.name.includes(this.searchName));
    },
    pagedData() {
      const start = (this.currentPage - 1) * this.pageSize;
      return this.filteredData.slice(start, start + this.pageSize);
    },
  },
  methods: {
    handleSearch() {
      this.currentPage = 1;
    },
    handleAdd() {
      this.dialogMode = "add";
      this.form = { id: null, name: "" };
      this.dialogVisible = true;
    },
    handleEdit(row) {
      this.dialogMode = "edit";
      this.form = { id: row.id, name: row.name };
      this.dialogVisible = true;
    },
    handleDialogConfirm() {
      if (!this.form.name) {
        ElMessage.warning("请输入用户名");
        return;
      }
      if (this.dialogMode === "add") {
        const today = new Date().toISOString().slice(0, 10);
        this.userList.unshift({
          id: Date.now(),
          date: today,
          name: this.form.name,
        });
        ElMessage.success("添加成功");
      } else {
        const target = this.userList.find((u) => u.id === this.form.id);
        if (target) target.name = this.form.name;
        ElMessage.success("修改成功");
      }
      this.dialogVisible = false;
    },
    handleDelete(row) {
      ElMessageBox.confirm(
        `确定要删除用户「${row.name}」吗？`,
        "提示",
        { type: "warning" }
      )
        .then(() => {
          this.userList = this.userList.filter((u) => u.id !== row.id);
          ElMessage.success("删除成功");
        })
        .catch(() => {});
    },
  },
};
</script>

<style scoped>
.toolbar {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
}
.toolbar-label {
  font-size: 14px;
  color: #333;
}
.toolbar-input {
  width: 240px;
}
.user-table {
  width: 100%;
}
.date-cell {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  color: #606266;
}
.date-icon {
  color: #909399;
}
.name-tag {
  background: #ecf5ff;
  color: #409eff;
  border: 1px solid #d9ecff;
}
.pagination {
  margin-top: 20px;
}
</style>
