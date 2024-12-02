<script setup>
import axios from 'axios';
import { ref,onMounted } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter()

const name = ref('')
const address = ref('')
const phone = ref('')
const email = ref('')
const nameError = ref('')
const addressError = ref('')
const phoneError = ref('')
const emailError = ref('')

let urlCarCartDetails = 'http://localhost/vuejs-server/api.php/cart-detail';
let urlCheckout = 'http://localhost/vuejs-server/api.php/check-out';

const cartDetail = ref([])
const callAPI = async ()=>{
    try {
        let user_id = JSON.parse(localStorage.getItem('userLogin')).id
        let respone = await axios.get(urlCarCartDetails + "?user_id="+user_id)
        if(respone.status ==200){
            cartDetail.value =  respone.data.cart_details
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

const handleSubmit = async ()=>{
    checkValidate()
    if(nameError.value == '' && emailError.value == '' && phoneError.value == '' && addressError.value == ''){
        try {
        let user_id = JSON.parse(localStorage.getItem('userLogin')).id
        let formData = new FormData()
        formData.append("user_id",user_id)
        formData.append("email",email.value)
        formData.append("name",name.value)
        formData.append("address",address.value)
        formData.append("phone",phone.value)

        let respone = await axios.post(urlCheckout,formData)
        if(respone.status ==200){
            alert('Thanh toán thành công')
            router.push('/don-hang')
        }
    } catch (error) {
        console.log(error);
        
    }
    }
}
const checkValidate = () => {
    nameError.value = name.value === '' ? 'Tên không được để trống' : '';
    addressError.value = address.value === '' ? 'Địa chỉ không được để trống' : '';
    phoneError.value = phone.value === '' ? 'Số điện thoại không được để trống' : '';
    emailError.value = email.value === '' ? 'Email không được để trống' : '';
}

</script>
<template>
        <div class="row main-website justify-content-center mt-5">
            <div class="col-10">
                <div class="container">
                    <div class="row">
                        <div class="col-7">
                            <h4>Thông tin thanh toán</h4>
                            <form @submit.prevent="handleSubmit">
                                <div class="mb-4">
                                    <label for="name">Name</label>
                                    <input type="text" placeholder="Name" class="form-control" v-model="name" @keyup="checkValidate('name')">
                                    <span class="text-danger" v-if="nameError !== ''">{{ nameError }}</span>

                                </div>
                                <div class="mb-4">
                                    <label for="address">Address</label>
                                    <input type="text" placeholder="Address" class="form-control" v-model="address" @keyup="checkValidate()">
                                    <span class="text-danger" v-if="addressError !== ''">{{ addressError }}</span>

                                </div>
                                <div class="mb-4">
                                    <label for="phone">Phone</label>
                                    <input type="text" placeholder="Phone" class="form-control" v-model="phone" @keyup="checkValidate()">
                                    <span class="text-danger" v-if="phoneError !== ''">{{ phoneError }}</span>

                                </div>
                                <div class="mb-4">
                                    <label for="name">Email</label>
                                    <input type="text" placeholder="Email" class="form-control" v-model="email" @keyup="checkValidate()">
                                    <span class="text-danger" v-if="emailError !== ''">{{ emailError }}</span>
                                </div>
                                <button class="btn btn-success">Xác nhận thanh toán</button>
                            </form>
                        </div>
                        <div class="col-5">
                            <h4>Danh sách sản phẩm</h4>
                            <table class="table">
                                <thead>
                                    <tr>
                                        <th>STT</th>
                                        <th>Tên sản phẩm</th>
                                        <th>Giá</th>
                                        <th>Số lượng</th>
                                        <th>Thành tiền</th>

                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(item,index) in cartDetail" :key="index">
                                        <td>{{ index +1 }}</td>
                                        <td>{{ item.product_name }}</td>
                                        <td>{{ convertPrice(Number(item.price)) }}</td>
                                        <td>{{ item.quantity }}</td>
                                        <td>{{ convertPrice(Number(Number(item.price) * Number(item.quantity)) )}} VND</td>

                                        
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>
</template>