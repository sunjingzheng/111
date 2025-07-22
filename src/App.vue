<script setup>
import { onMounted, ref } from 'vue';
import Edit from './components/Edit.vue'
import axios from 'axios';
// TODO: 列表渲染
//建立变量  获取列表数据  进行绑定   渲染到页面
//声明响应式list->调用接口获取数据->后端数据给list->绑定到table组件
const list=ref([

])
const getlist =async()=>{
  const res=await axios.get('/list');
  list.value=res.data;
}

onMounted(()=>{
  getlist();
})



// TODO: 删除功能
//获取当前ID   通过ID调用接口   更新最新的列表
const del=async(id)=>{
  console.log(id);
  await axios.delete('/del/${id}')
  getlist();
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
        <template #default="{row}">
          <el-button type="primary" link>编辑</el-button>
          <el-button type="danger" @click="del(row.id)" link>删除</el-button>
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
