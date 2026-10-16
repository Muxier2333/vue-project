<template>
  <el-container class="layout-container">
    <!-- 顶部标题栏 -->
    <el-header class="app-header">
      <span class="app-title">Quiz后台管理</span>
    </el-header>

    <el-container>
      <!-- 左侧菜单 -->
      <el-aside width="200px" class="app-aside">
        <div class="menu-group">
          <div class="menu-group-title" @click="collapseOpen = !collapseOpen">
            <el-icon><Setting /></el-icon>
            <span>管理选项</span>
            <el-icon class="menu-arrow">
              <ArrowDown v-if="collapseOpen" />
              <ArrowRight v-else />
            </el-icon>
          </div>
          <transition name="fold">
            <div v-show="collapseOpen" class="menu-sub-list">
              <div
                class="menu-sub-item"
                :class="{ active: activeMenu === 'user' }"
                @click="activeMenu = 'user'"
              >
                用户管理
              </div>
              <div
                class="menu-sub-item"
                :class="{ active: activeMenu === 'question' }"
                @click="activeMenu = 'question'"
              >
                题目管理
              </div>
            </div>
          </transition>
        </div>
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
import { Setting, ArrowDown, ArrowRight } from "@element-plus/icons-vue";

export default {
  components: { UserManage, QuestionManage, Setting, ArrowDown, ArrowRight },
  data() {
    return {
      activeMenu: "user",
      collapseOpen: true,
    };
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
  padding-top: 10px;
}
.menu-group-title {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 0 20px;
  height: 40px;
  font-size: 14px;
  color: #333;
  cursor: pointer;
  user-select: none;
}
.menu-group-title:hover {
  background: #ecf0f5;
}
.menu-arrow {
  margin-left: auto;
  font-size: 12px;
}
.menu-sub-list {
  overflow: hidden;
}
.menu-sub-item {
  padding: 0 20px 0 44px;
  height: 40px;
  line-height: 40px;
  font-size: 14px;
  color: #333;
  cursor: pointer;
}
.menu-sub-item:hover {
  background: #ecf0f5;
}
.menu-sub-item.active {
  color: #409eff;
}
.fold-enter-active,
.fold-leave-active {
  transition: all 0.2s ease;
}
.fold-enter-from,
.fold-leave-to {
  opacity: 0;
  transform: translateY(-6px);
}
.app-main {
  background: #fff;
  padding: 24px 30px;
}
</style>
