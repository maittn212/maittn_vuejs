<script setup>
let url = 'http://localhost/vuejs-server/api.php/categories'
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { RouterLink } from 'vue-router';

const listCategory = ref([])
onMounted(async () => {
       let respone = await axios.get(url)
       listCategory.value = respone.data
       // console.log(respone.data)
})
const handleDelete = async (idCategory) => {
       const check = confirm('Bạn có muốn xóa không?')
       if (check) {
              
              let respone = await axios.delete(url + "/" + idCategory)
              if (respone) {
                     alert(respone.data.message)
                     let respone2 = await axios.get(url)
                     listCategory.value = respone2.data
              }
       }
}
</script>

<template>
       <div class="p-4" style="min-height: 800px;">
              <h1>Quản lý danh mục</h1>
              <RouterLink to="/admin/add-category" class="btn btn-primary">Add Category</RouterLink>
              <table class="table">
                     <thead>
                            <tr>
                                   <th>STT</th>
                                   <th>Name</th>
                                   <th>Description</th>
                                   <th>Action</th>
                            </tr>
                     </thead>
                     <tbody>
                            <tr v-for="(category, index) in listCategory" :key="index">
                                   <td>{{ index + 1 }}</td>
                                   <td>{{ category.name }}</td>
                                   <td>{{ category.description }}</td>
                                   <td>
                                          <RouterLink :to="`/admin/update-category/${category.id}`"
                                                 class="btn btn-warning">Sửa</RouterLink>
                                          <button class="btn btn-danger" @click="handleDelete(category.id)">Xóa</button>
                                   </td>
                            </tr>
                     </tbody>
              </table>
       </div>

</template>