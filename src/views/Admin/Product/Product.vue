<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';
import axios from 'axios';

let urlAPI = "http://localhost/vuejs-server/api.php/products"
let urlImage = 'http://localhost/vuejs-server/uploads/product/'

const productList = ref('')

onMounted(async () => {
       try {
              let respone = await axios.get(urlAPI)
              productList.value = respone.data
              // console.log(productList.value);
       } catch (error) {
              console.log('Loi');
       }
})

const callAPI = async ()=>{

}
const convertPrice = (number) => {
       return number.toLocaleString('vi-VN')
}
const handleClick = async (id)=>{
       let check = confirm('Bạn có muốn xóa không?')
       if(check){
              try {
                     let respone = await axios.delete(urlAPI + '/' + id)
                     if(respone.status ===200){
                            alert("Xóa thành công")
                            // Hiển thị lại danh sách sản phẩm
                            let respone2 = await axios.get(urlAPI)
                            productList.value = respone2.data
                     }
              } catch (error) {
                     console.log('Call API lỗi');
                     
              }
       }
}
</script>

<template>
       <div class="p-4" style="min-height: 800px;">
              <h1>Quản lý sản phẩm</h1>
              <RouterLink to="/admin/add-product" class="btn btn-primary">Thêm mới</RouterLink>
              <table class="table">
                     <thead>
                            <tr>
                                   <th>STT</th>
                                   <th>Name</th>
                                   <th>Category</th>
                                   <th>Description</th>
                                   <th>Price</th>
                                   <th>Image</th>
                                   <th>Action</th>
                            </tr>
                     </thead>
                     <tbody>
                            <tr v-for="(product, index) in productList" :key="index">
                                    <td>{{ index +1 }}</td>
                                    <td>{{ product.name }}</td>
                                    <td>{{ product.categoryName }}</td>
                                    <td>{{ product.description }}</td>
                                   <td>{{ convertPrice(Number( product.price )) }}</td>
                                   <td>
                                          <img v-if="product.image != null" :src="urlImage + product.image" alt="" width="50">
                                          <span v-else class="badge text-bg-warning">Không có ảnh</span>
                                   </td>
                                   <td>
                                          <RouterLink :to="`/admin/update-product/${product.id}`" class="btn btn-warning">Sửa</RouterLink>
                                          <button @click="handleClick(product.id)" class="btn btn-danger">Xóa</button>
                                          
                                   </td>
                            </tr>
                     </tbody>
              </table>



       </div>


</template>