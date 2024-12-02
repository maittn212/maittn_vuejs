<script setup>
    let urlProduct = 'http://localhost/vuejs-server/api.php/products';
    let urlCategory = 'http://localhost/vuejs-server/api.php/categories';
    let urlImage = 'http://localhost/vuejs-server/uploads/product/'
    import { ref,onMounted, computed, watch } from 'vue';
    import { RouterLink } from 'vue-router';
    import axios from 'axios';
    import { useRoute } from 'vue-router';
// Product
const route = useRoute()
    const listProduct = ref([])
    const callAPI = async ()=>{
        try {
            let respone = await axios.get(urlProduct + '?idCategory=' +route.params.idCategory)
            console.log(respone.data);
            listProduct.value = respone.data
            
        } catch (error) {
            console.log('Call API loi');   
        }
    }
    const convertPrice = (number)=>{
        return number.toLocaleString('vi-VN')
    }
// Category
    let listCategory = ref([])
    const callAPICategory = async()=>{
        try {
            let respone = await axios.get(urlCategory)
            listCategory.value = respone.data
        } catch (error) {
            console.log('Call API loi');
            
        }
    }
    
    onMounted(()=>{
        callAPI();
        callAPICategory();
    })
    watch(
        ()=> route.params.idCategory, newIdCategory =>{
            callAPI();
        }
    )


</script>fds

<template>
    <div class="row main-website">
        <div class="col-8">
            <div class="d-flex flex-wrap" v-if="listProduct.length >0">
                <!-- Sản phẩm  -->
                <div  class="card card-product" v-for="(product,index) in listProduct" :key="index">
                    <img v-if="product.image !== null"  :src="urlImage + product.image" class="card-img-top" :alt="product.name">
                    <img v-else src="../../assets/images/OIP.jpg" class="card-img-top" :alt="product.name">


                    <div class="card-body">
                        <h5 class="card-title">{{ product.name }}</h5>
                        <p class="m-0 p-0 fw-bold text-danger">{{ convertPrice(Number(product.price)) }} VNĐ</p>
                        <p class="card-text">{{ product.description }}</p>
                        <a href="#" class="btn btn-primary">Xem chi tiết</a>
                    </div>
                </div>

               
            </div>
            <div v-else class="alert alert-danger col-12 mt-3" role="alert">
                    <p class="text-center">Không có sản phẩm</p>
                </div>
            <!-- Sản phẩm  -->

        </div>
        <div class="col-3 offset-1 p-5">
            <div class="card">
                <div class="card-header">
                    Danh sách danh mục
                </div>
                <div class="list-group">
                    <RouterLink :to="`/san-pham/${category.id}`" class="list-group-item list-group-item-action" :class="{active: category.id == route.params.idCategory}" v-for="(category,index) in listCategory" :key="index">{{ category.name }}</RouterLink>
                </div>
            </div>
        </div>

    </div>
</template>

<style lang="scss" scoped>
    .card-product{
        width: 30%;
        margin: 10px;
        margin-top:30px ;
    }

</style>