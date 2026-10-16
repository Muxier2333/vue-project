<template>
  <div class="question-manage">
    <!-- 搜索操作行 -->
    <div class="toolbar">
      <span class="toolbar-label">题目</span>
      <el-input
        v-model="searchKey"
        placeholder="请输入题目关键词"
        class="toolbar-input"
        clearable
      ></el-input>
      <el-button type="primary" @click="handleSearch">查询</el-button>
      <el-button type="success" @click="handleAdd">添加题目</el-button>
    </div>

    <!-- 题目表格 -->
    <el-table :data="pagedData" class="question-table" border>
      <el-table-column prop="index" label="序号" width="80">
        <template #default="{ $index }">
          {{ (currentPage - 1) * pageSize + $index + 1 }}
        </template>
      </el-table-column>
      <el-table-column prop="title" label="题目" width="240">
        <template #default="{ row }">
          <el-tag class="title-tag">{{ row.title }}</el-tag>
        </template>
      </el-table-column>
      <el-table-column prop="options" label="选项" min-width="240">
        <template #default="{ row }">
          <span>{{ row.options.join("，") }}</span>
        </template>
      </el-table-column>
      <el-table-column prop="answer" label="答案" width="160">
        <template #default="{ row }">
          <span>{{ row.answer }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="200">
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
      :title="dialogMode === 'add' ? '添加题目' : '编辑题目'"
      width="500px"
    >
      <el-form label-width="80px">
        <el-form-item label="题目">
          <el-input v-model="form.title" placeholder="请输入题目内容"></el-input>
        </el-form-item>
        <el-form-item label="选项A">
          <el-input v-model="form.optA" placeholder="选项A"></el-input>
        </el-form-item>
        <el-form-item label="选项B">
          <el-input v-model="form.optB" placeholder="选项B"></el-input>
        </el-form-item>
        <el-form-item label="选项C">
          <el-input v-model="form.optC" placeholder="选项C"></el-input>
        </el-form-item>
        <el-form-item label="选项D">
          <el-input v-model="form.optD" placeholder="选项D"></el-input>
        </el-form-item>
        <el-form-item label="答案">
          <el-select v-model="form.answer" placeholder="请选择正确答案">
            <el-option label="A" value="A"></el-option>
            <el-option label="B" value="B"></el-option>
            <el-option label="C" value="C"></el-option>
            <el-option label="D" value="D"></el-option>
          </el-select>
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
import { ElMessageBox, ElMessage } from "element-plus";

export default {
  data() {
    return {
      searchKey: "",
      currentPage: 1,
      pageSize: 4,
      dialogVisible: false,
      dialogMode: "add",
      form: {
        id: null,
        title: "",
        optA: "",
        optB: "",
        optC: "",
        optD: "",
        answer: "",
      },
      questionList: [
        {
          id: 1,
          title: "法国的首都是哪个城市？",
          options: ["巴黎", "伦敦", "柏林", "罗马"],
          answer: "巴黎",
        },
        {
          id: 2,
          title: "法国的首都是哪个城市？",
          options: ["巴黎", "伦敦", "柏林", "罗马"],
          answer: "巴黎",
        },
        {
          id: 3,
          title: "法国的首都是哪个城市？",
          options: ["巴黎", "伦敦", "柏林", "罗马"],
          answer: "巴黎",
        },
        {
          id: 4,
          title: "法国的首都是哪个城市？",
          options: ["巴黎", "伦敦", "柏林", "罗马"],
          answer: "巴黎",
        },
        {
          id: 5,
          title: "中国的首都是哪个城市？",
          options: ["上海", "北京", "广州", "深圳"],
          answer: "北京",
        },
        {
          id: 6,
          title: "日本的首都是哪个城市？",
          options: ["大阪", "京都", "东京", "名古屋"],
          answer: "东京",
        },
      ],
    };
  },
  computed: {
    filteredData() {
      if (!this.searchKey) return this.questionList;
      return this.questionList.filter((q) => q.title.includes(this.searchKey));
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
      this.form = {
        id: null,
        title: "",
        optA: "",
        optB: "",
        optC: "",
        optD: "",
        answer: "",
      };
      this.dialogVisible = true;
    },
    handleEdit(row) {
      this.dialogMode = "edit";
      this.form = {
        id: row.id,
        title: row.title,
        optA: row.options[0],
        optB: row.options[1],
        optC: row.options[2],
        optD: row.options[3],
        answer: row.answer,
      };
      this.dialogVisible = true;
    },
    handleDialogConfirm() {
      if (!this.form.title) {
        ElMessage.warning("请输入题目内容");
        return;
      }
      const options = [this.form.optA, this.form.optB, this.form.optC, this.form.optD];
      const answerMap = { A: this.form.optA, B: this.form.optB, C: this.form.optC, D: this.form.optD };
      const answerText = answerMap[this.form.answer] || this.form.answer;

      if (this.dialogMode === "add") {
        this.questionList.unshift({
          id: Date.now(),
          title: this.form.title,
          options,
          answer: answerText,
        });
        ElMessage.success("添加成功");
      } else {
        const target = this.questionList.find((q) => q.id === this.form.id);
        if (target) {
          target.title = this.form.title;
          target.options = options;
          target.answer = answerText;
        }
        ElMessage.success("修改成功");
      }
      this.dialogVisible = false;
    },
    handleDelete(row) {
      ElMessageBox.confirm(`确定要删除题目「${row.title}」吗？`, "提示", {
        type: "warning",
      })
        .then(() => {
          this.questionList = this.questionList.filter((q) => q.id !== row.id);
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
  width: 260px;
}
.question-table {
  width: 100%;
}
.title-tag {
  background: #ecf5ff;
  color: #409eff;
  border: 1px solid #d9ecff;
}
.pagination {
  margin-top: 20px;
}
</style>
