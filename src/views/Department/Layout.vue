<script lang="ts" setup>
import { ref, watch,  } from 'vue';
import { ArrowRight } from "@element-plus/icons-vue"
import { useRouter } from "vue-router"
const { currentRoute } = useRouter();  

const breadcrumbItems = ref([]);

watch(  
  () => currentRoute.value,
  (toRoute, fromRoute) => {  
    // 通常，我们只需要toRoute.matched，因为它包含了当前路由及其所有父路由的信息  
    toRoute.matched.splice(2, 1);   // 去掉第2层路由
    toRoute.matched[1].path = "/department/dept/introduction";  // 手动修改第2层路由为，默认路由
    breadcrumbItems.value = toRoute.matched.map(item => ({
      path: item.path,  
      meta: item.meta || {}, // 确保meta存在，避免undefined  
    }));
  },  
  { immediate: true } // 立即执行一次回调函数  
);  
</script>

<template>
  <div class="common-layout">
    <img src="../../assets/bg1.jpg" width="100%">

    <el-container>
      <el-header>
        <el-breadcrumb :separator-icon="ArrowRight">
          <el-breadcrumb-item
            v-for="(item, i) in breadcrumbItems"
            :key="i"
            :to="{ path: item.path }"
          >
            {{ item.meta.title }}
          </el-breadcrumb-item>
        </el-breadcrumb>
      </el-header>
      <el-container>
        <!-- 左侧菜单栏 -->
        <el-aside width="200px">
          <h2 class="mb-2">系部概况</h2>
          <el-menu
            :default-active="$router.currentRoute.value.path"
            background-color="#f6f6f6"
            class="el-menu-vertical-demo"
            router
          >
            <el-menu-item index="/department/dept/introduction">
              <span>本系简介</span>
            </el-menu-item>
            <el-menu-item index="/department/dept/leadership">
              <span>领导简介</span>
            </el-menu-item>
            <el-menu-item index="/department/dept/profession">
              <span>专业设置</span>
            </el-menu-item>
            <el-menu-item index="/department/dept/achievement">
              <span>十年成果展</span>
            </el-menu-item>
          </el-menu>
        </el-aside>

        <!-- 右侧内容面板 -->
        <el-main>
          <router-view></router-view>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<style scoped>
h2 {
  text-align: center;
  font-weight: 500;
}

/* 面包屑右浮动 */
.el-breadcrumb {
    float: right
}

.el-container {
    width: 70%;
    margin: 0 auto;
}

.el-aside {
  border-top: 10px solid #015ca1;
}

/* 菜单栏中文字居中 */
.el-menu-item * {
  margin: 0 auto;
}

/* 菜单选中时样式 */
.is-active {
  color: #fff;
  background-color: #fa9d0a;
}

/* 鼠标悬停时样式 */
.el-menu-item:hover {
  color: #fff;
  background-color: #fa9d0a;
}
</style>