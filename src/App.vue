<script setup>
import Edit from './components/Edit.vue'
import axios from 'axios'
import { ref, onMounted } from 'vue'
// TODO: 列表渲染
// 声明一个响应数据list -> 调接口 -> 后端数据赋值给list -> 绑定到table组件
const list = ref([])
const getList = async () => {
  // 调用查询接口
  const res = await axios.get('/list')
  list.value = res.data
}
onMounted(() => getList())
// TODO: 删除功能
// 获取当前行的id-> 通过id调用删除接口-> 更新最新的列表
const onDelete = async (id) => {
  console.log(id)
  // 调用删除接口
  await axios.delete(`/del/${id}`)
  // 刷新数据
  getList()
}
// TODO: 编辑功能
</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <!-- 作用域插槽：row获取表格props信息 -->
        <template #default="{ row }">
          <el-button type="primary" link>编辑</el-button>
          <el-button type="danger" @click="onDelete(row.id)" link
            >删除</el-button
          >
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
