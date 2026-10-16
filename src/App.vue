<template>
  <el-container class="layout-container">
    <!-- 顶部标题栏 -->
    <el-header class="app-header">
      <span class="app-title">Quiz后台管理</span>
    </el-header>

    <el-container>
      <!-- 左侧菜单 -->
      <el-aside width="200px" class="app-aside">
        <div class="aside-group-title">管理选项</div>
        <el-menu
          :default-active="activeMenu"
          class="aside-menu"
          @select="handleSelect"
        >
          <el-menu-item index="user">
            <span>用户管理</span>
          </el-menu-item>
          <el-menu-item index="question">
            <span>题目管理</span>
          </el-menu-item>
        </el-menu>
      </el-aside>

      <!-- 右侧内容 -->
      <el-main class="app-main">
        <user-manage v-if="activeMenu === 'user'" />
        <question-manage v-else />
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import UserManage from "./views/element/UserManage.vue";
import QuestionManage from "./views/element/QuestionManage.vue";

export default {
  components: { UserManage, QuestionManage },
  data() {
    return {
      activeMenu: "user",
    };
  },
  methods: {
    handleSelect(index) {
      this.activeMenu = index;
    },
  },
};
</script>

<style>
html,
body,
#app {
  margin: 0;
  padding: 0;
  height: 100%;
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Microsoft YaHei",
    Arial, sans-serif;
}
.layout-container {
  height: 100%;
}
.app-header {
  background: linear-gradient(to bottom, #d8dce3, #eef0f4);
  padding: 0 20px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #dcdfe6;
}
.app-title {
  font-size: 28px;
  font-weight: 500;
  color: #222;
}
.app-aside {
  background: #f5f7fa;
  border-right: 1px solid #e4e7ed;
  min-height: calc(100% - 60px);
}
.aside-group-title {
  font-size: 14px;
  color: #333;
  padding: 18px 20px 10px;
}
.aside-menu {
  border-right: none;
  background: transparent;
}
.aside-menu .el-menu-item {
  padding-left: 40px !important;
  color: #333;
}
.aside-menu .el-menu-item.is-active {
  color: #409eff;
  background: transparent;
}
.app-main {
  background: #fff;
  padding: 24px 30px;
}
</style>
