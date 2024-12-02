<script setup>
import { reactive, ref, onMounted,computed } from 'vue';
import { RouterLink } from 'vue-router';
import axios from 'axios';
let url = 'http://localhost/vuejs-server/api.php/cart-detail';
let urlCartUpdate = 'http://localhost/vuejs-server/api.php/cart-update';


const cart = ref({})
const cartDetail = ref([])
const callAPI = async () => {
    let user_id = JSON.parse(localStorage.getItem('userLogin')).id
    try {
        let respone = await axios.get(url + "/?user_id=" + user_id)
        if(respone.data.status){
            cart.value = respone.data.cart
            cartDetail.value = respone.data.cart_details
        }
        // console.log(cartDetail);
        
    } catch (error) {
        console.log(error);
    }
}
onMounted(() => {
    callAPI()
})
const convertPrice = (number) => {
    return number.toLocaleString('vi-VN')
}
const totalPrice = computed(()=>{
    let total = 0
    cartDetail.value.forEach(item=>{
        total = total + (item.price * item.quantity)
    })
    return total
})

const handleUpdateCart = async(act, product_id)=>{
    try {
        let link = urlCartUpdate +"?cart_id=" +cart.value.id +"&action=" +act + "&product_id="+ product_id
        let respone = await axios.get(link)
        if(respone.status == 200){
            alert('Cập nhật giỏ hàng thành công')
            callAPI()
        }

    } catch (error) {
        console.log(error);
        
    }
}

</script>
<template>
        <div class="row main-website justify-content-center mt-5">
            <div class="col-8">
                <h4>Giỏ hàng</h4>
    <table class="table">
        <thead>
            <tr>
                <th>STT</th>
                <th>Tên sản phẩm</th>
                <th>Giá sản phẩm</th>
                <th>Số lượng</th>
                <th>Thành tiền</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody>
            <tr v-if="cartDetail.length ==0">
                <td colspan="6">Không có sản phẩm trong giỏ hàng
                    <RouterLink to="/">Tiếp tục mua hàng</RouterLink>
                </td>
            </tr>
            <tr v-else v-for="(item,index) in cartDetail" :key="index">
                <td>{{ index +1 }}</td>
                <td>{{ item.product_name }}</td>
                <td>{{ convertPrice(Number( item.price )) }}</td>
                <td>
                    <div class="d-flex">
                        <button class="btn btn-sm btn-info" @click="handleUpdateCart('decrease',item.product_id)">-</button>
                        <span>{{ item.quantity }}</span>
                        <button class="btn btn-sm btn-info" @click="handleUpdateCart('increase',item.product_id)">+</button>
                    </div>
                </td>
                <td>{{ convertPrice(Number(Number(item.price) * Number(item.quantity)) )}} VND</td>
                <td>
                    <button @click="handleUpdateCart('delete',item.product_id)" class="btn btn-danger">Xóa</button>
                </td>
            </tr>
            <tr v-if="cartDetail.length != 0">
                <td colspan="4"></td>
                <td>Tổng tiền {{ convertPrice(totalPrice) }} VND</td>
                <td>
                    <router-link class="btn btn-success" to="/thanh-toan">Thanh toán</router-link>
                </td>
            </tr>
        </tbody>
    </table>
 </div>
 </div>
</template>