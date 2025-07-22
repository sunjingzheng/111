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
  await axios.delete(`/del/${id}`)
  getlist();
}
const editRef=ref(null);
const onEdit=(row)=>{
  // @ts-ignore
  editRef.value.openEdit(row);
}
// TODO: 编辑功能
//打开弹窗  回填数据  更新数据
//1.打开弹窗 （获取子组件实例 调用方法或修改属性）
//2.回填数据  （根据当前行数据  回填到弹窗中）
//3.更新数据  （点击确认按钮  调用接口  更新数据）


</script>

<template>
  <div class="app">
    <el-table :data="list">
      <el-table-column label="ID" prop="id"></el-table-column>
      <el-table-column label="姓名" prop="name" width="150"></el-table-column>
      <el-table-column label="籍贯" prop="place"></el-table-column>
      <el-table-column label="操作" width="150">
        <template #default="{row}">
          <el-button type="primary" @click="onEdit(row) ">编辑</el-button>
          <el-button type="danger" @click="del(row.id)" link>删除</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
  <Edit ref="editRef" @refresh="getlist" />
</template>

<style scoped>
.app {
  width: 980px;
  margin: 100px auto 0;
}
</style>
