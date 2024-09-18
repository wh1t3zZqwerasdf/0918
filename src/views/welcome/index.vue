<script setup lang="ts">
import { usePermissionStoreHook } from "@/store/modules/permission";
import { computed, ref } from "vue";

const tableData = computed(() => {
  return usePermissionStoreHook().$state.dynamicRoutes;
});

const handleClick = () => {
  console.log("click");
};

const expandedRows = ref([]);

const handleExpandChange = (row, expanded) => {
  if (expanded) {
    // 添加到展开数组
    expandedRows.value.push(row.id);
  } else {
    // 从展开数组中移除
    const index = expandedRows.value.indexOf(row.id);
    if (index !== -1) {
      expandedRows.value.splice(index, 1);
    }
  }
};
</script>

<template>
  <div>
    <el-table
      :data="tableData"
      style="width: 100%; margin-bottom: 20px"
      row-key="path"
      border
      :expand-row-keys="expandedRows"
      @expand-change="handleExpandChange"
    >
      <el-table-column prop="meta.title" label="标题">
        <template v-slot="scope">
          <span
            :class="
              scope.row.children && scope.row.children.length > 0
                ? 'text-green-500 text-base'
                : 'text-orange-500'
            "
          >
            {{ scope.row.meta.title }}</span
          >
        </template>
      </el-table-column>
      <el-table-column prop="path" label="基础路径">
        <template v-slot="scope">
          {{ scope.row.path }}
        </template>
      </el-table-column>
      <el-table-column prop="name" label="路由Name" />
      <el-table-column prop="meta.icon" label="图标" />
      <el-table-column fixed="right" label="操作" min-width="80">
        <template #default>
          <el-button link type="primary" size="small">编辑</el-button>
          <el-button link type="primary" size="small">添加</el-button>
          <el-button link type="primary" size="small" @click="handleClick">
            删除
          </el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<style lang="scss" scoped></style>
