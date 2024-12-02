<script setup>
import axios from 'axios';
import { ref, onMounted } from 'vue';
import { useRoute, useRouter } from 'vue-router';
let urlProductDetail = 'http://localhost/vuejs-server/api.php/product-detail';
let urlAddCart = 'http://localhost/vuejs-server/api.php/cart';
let urlImage = 'http://localhost/vuejs-server/uploads/product/'


const route = useRoute();
const router = useRouter();
const id = ref(route.params.id)
const product = ref({})

const callAPI = async () => {
    try {
        let url = urlProductDetail + "?product_id=" + route.params.id
        let respone = await axios.get(url)
        if (respone.data.status) {
            product.value = respone.data.products
        }
    } catch (error) {
        console.log(error);

    }
}

onMounted(() => {
    callAPI();
})
const convertPrice = (number) => {
    return number.toLocaleString('vi-VN')
}

const quantity = ref(1)
const addCart = async () => {
    if(quantity.value >=1){
        try {
            let url = urlAddCart + '/' + '?product_id=' + product.value.id + "&quantity=" + quantity.value + "&user_id=" + JSON.parse(localStorage.getItem('userLogin')).id
            let respone = await axios.get(url)

            alert(respone.data.message)
            router.push('/giohang')
            
        } catch (error) {
            console.log(error);
            
        }
    }

}
</script>
<template>
    <div class="row main-website justify-content-center mt-5">
        <div class="col-8">
            <h4 class="mb-5">Chi tiết sản phẩm</h4>
            <div class="container">
                <div class="row">
                    <div class="col-3">
                        <img v-if="product.image !== null" :src="urlImage + product.image" alt="" class="img-product" />
                        <img v-else src="../../assets/images/OIP.jpg" alt="" class="img-product" />

                    </div>
                    <div class="col-9">
                        <p><span class="fw-bold">Tên sản phẩm:</span> {{ product.name }}</p>
                        <p><span class="fw-bold">Danh mục:</span> {{ product.categoryName }}</p>
                        <p><span class="fw-bold">Giá sản phẩm:</span> {{ convertPrice(Number(product.price)) }} VNĐ</p>
                        <p><span class="fw-bold">Mô tả: </span> {{ product.description }}</p>
                        <p>
                            <span class="fw-bold">Số lượng </span>
                            <input type="text" v-model="quantity" class="form-control">
                        </p>

                        <button class="btn btn-success" @click="addCart">Thêm vào giỏ hàng</button>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.img-product {
    width: 100%;
    object-fit: cover;
}
</style>