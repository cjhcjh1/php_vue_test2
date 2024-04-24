<template>
  <el-container style="height: 90vh;">
    <!-- 头部（可选） -->
    <!-- <el-header style="background-color: #f5f7fa; padding: 0 20px;">
      <div>Header Content</div>
    </el-header> -->

    <el-container>
      <!-- 左侧菜单 -->
      <el-aside width="200px" style="background-color: #545c64; color: #fff;">
        <el-menu default-active="1" @select="handleSelect" background-color="#545c64" text-color="#fff"
          active-text-color="#ffd04b">
          <el-menu-item v-for="(item, index) in menuItems" :key="index" :index="index.toString()">
            {{ item.name }}
          </el-menu-item>
        </el-menu>
      </el-aside>

      <!-- 主要内容区域 -->
      <el-main>
        <!-- 子菜单和内容应该放在这里 -->
        <el-container>
          <!-- 子菜单放置在右侧 -->
          <el-header style="text-align: right;">
            <el-menu v-if="activeSubMenu" mode="horizontal" @select="handleSubSelect">
              <el-menu-item v-for="(subItem, subIndex) in activeSubMenu" :key="subIndex" :index="subIndex.toString()">
                {{ subItem.title }}
              </el-menu-item>
            </el-menu>
          </el-header>

          <!-- 内容区域 -->
          <el-main>
            <div v-if="activeSubMenuContent">
              <h3>选中的内容：</h3>
              <p>{{ activeSubMenuContent }}</p>
            </div>
          </el-main>
        </el-container>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  setup() {
    const activeIndex = ref('0');
    const subMenuActiveIndex = ref(0);
    const menuItems = ref([
      { name: '菜单1', subMenu: [{ title: '菜单1-1' }, { title: '菜单1-2' }, { title: '菜单1-3' }] },
      { name: '菜单2', subMenu: [{ title: '菜单2-1' }, { title: '菜单2-2' }, { title: '菜单2-3' }] },
      { name: '菜单3', subMenu: [{ title: '菜单3-1' }, { title: '菜单3-2' }, { title: '菜单3-3' }] }
    ]);

    const activeSubMenu = computed(() => {
      const selectedMenuItem = menuItems.value[Number(activeIndex.value)];
      console.log(selectedMenuItem, Number(activeIndex.value))
      return selectedMenuItem ? selectedMenuItem.subMenu : [];
    });

    const activeSubMenuContent = computed(() => {
      const selectedSubMenu = activeSubMenu.value[subMenuActiveIndex.value];
      // console.log(selectedSubMenu)
      return selectedSubMenu ? selectedSubMenu.title : '';
    });

    function handleSelect(index) {
      activeIndex.value = index;
      subMenuActiveIndex.value = 0; // Reset sub-menu selection when main menu changes
    }

    function handleSubSelect(index) {
      subMenuActiveIndex.value = index;
    }

    return {
      activeIndex,
      subMenuActiveIndex,
      menuItems,
      activeSubMenu,
      activeSubMenuContent,
      handleSelect,
      handleSubSelect
    };
  }
};
</script>

<style scoped>
/* Add your styles here if needed */
</style>
