<script setup>
import axios from 'axios';
import { ref } from 'vue';

// 编辑
const dialogVisible = ref(false);
const form = ref({
  name: "",
  place: "",
});

// 打开
const open = ({name, place, id}) => {
  dialogVisible.value = true
  form.value.name = name
  form.value.place = place
  form.value.id = id
}
defineExpose({ open })

// 提交
const emit = defineEmits(['on-success'])
const onSubmit = async () => {
  if (form.value.name && form.value.place) {
    await axios.patch(`/edit/${form.value.id}`, form.value)
    dialogVisible.value = false
    emit('on-success')
  }
}
</script>

<template>
  <el-dialog v-model="dialogVisible" title="编辑" width="400px">
    <el-form :model="form" label-width="50px">
      <el-form-item label="姓名">
        <el-input v-model="form.name" placeholder="请输入姓名"/>
      </el-form-item>
      <el-form-item label="籍贯">
        <el-input v-model="form.place" placeholder="请输入籍贯"/>
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogVisible = false">取消</el-button>
        <el-button type="primary" @click="onSubmit">确认</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<style scoped>
.el-input {
  width: 290px;
}
</style>
