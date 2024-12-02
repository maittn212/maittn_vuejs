<script setup>
import axios from 'axios';
import { useRoute, useRouter } from 'vue-router';
import { ref,onMounted } from 'vue';
const route = useRoute()
const orderDetail = ref([])
let urlDetail = 'http://localhost/vuejs-server/api.php/order_detail';

const callAPI = async()=>{
    try {
        let respone = await axios.get(urlDetail +"?order_id=" +route.params.order_id)
        if(respone.status == 200){
            orderDetail.value = respone.data
            // console.log(orderDetail);
            
        }
    } catch (error) {
        console.log(error);
        
    }
}

onMounted(()=>{
    callAPI()
})
const convertPrice = (number) => {
    return number.toLocaleString('vi-VN')
}

</script>
<template>
       <div class="p-4" style="min-height: 800px;">

            <h4>Danh sách sản phẩm</h4>
            <table class="table">
                <thead>
                    <tr>
                        <th>STT</th>
                        <th>Tên sản phẩm</th>
                        <th>Giá</th>
                        <th>Số lượng</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item,index) in orderDetail" :key="index">
                        <td>{{ index +1 }}</td>
                        <td>
                           <RouterLink target="_blank" :to="`/san-pham-chi-tiet/${item.product_id}`">{{ item.product_name }}</RouterLink>
                        </td>
                        <td>{{ convertPrice(item.price) }} VND</td>
                        <td>{{ item.quantity }}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    
    
</template>