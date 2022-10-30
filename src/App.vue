<script setup>
import axios from "axios";
import { onMounted, ref } from "vue";
import Edit from "./components/Edit.vue";

// 列表
const list = ref([]);
const getList = async () => {
  const res = await axios.get("/list");
  list.value = res.data;
};
onMounted(() => getList());

// 删除
const onDelete = async (id) => {
  await axios.delete(`/del/${id}`);
  getList();
};

// 编辑
const edit = ref(null)
const onEdit = (item) => {
  edit.value.open(item)
}
const onEditSuc = () => {
  getList()
}
</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{ row }">
          <el-button type="primary" link @click="onEdit(row)">编辑</el-button>
          <el-button type="danger" link @click="onDelete(row.id)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit ref="edit" @on-success="onEditSuc" />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
