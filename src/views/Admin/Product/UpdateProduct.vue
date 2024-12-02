<script setup>
import { ref,onMounted } from 'vue';
import axios from 'axios';
import { useRouter,useRoute } from 'vue-router';

let urlAPI = 'http://localhost/vuejs-server/api.php/products'
let urlImage = 'http://localhost/vuejs-server/uploads/product/'
let urlAPICategory = 'http://localhost/vuejs-server/api.php/categories'


const router = useRouter()
const route = useRoute()

const name = ref('')
const description = ref('')
const price = ref('')
const image = ref(null)
const imageOldUrl = ref('')
const category_id = ref("")


const nameError = ref('')
const descriptionError = ref('')
const priceError = ref('')
const imageError = ref('')
const categoryError = ref('')

const callAPIProduct = async ()=>{
    try {
        let respone = await axios.get(urlAPI + "/" + route.params.id)
        name.value = respone.data.name
        description.value = respone.data.description
        price.value = respone.data.price
        imageOldUrl.value = respone.data.image
        category_id.value = respone.data.category_id


    } catch (error) {
        console.log('ERR');
        
    }
}


let listCategory = ref([])
const callAPTCate = async ()=>{
    const respone = await axios.get(urlAPICategory)
    listCategory.value = respone.data
}
onMounted(()=>{
    callAPTCate();
    callAPIProduct();
})

const handleChange = (event) => {
    const file = event.target.files[0]
    const maxFFileSize = 10*1024*1024 // Không quá 10MB
    if(file.size > maxFFileSize){
        imageError.value = "Kích thước file vượt quá dung lượng"
        image.value = null
    }
    image.value = file
}
const handleSubmit = async () => {
    checkEmpty('name')
    checkEmpty('description')
    checkEmpty('price')
    checkEmpty('category_id')

    if (nameError.value == '' && descriptionError.value == '' && priceError.value == '' && categoryError.value == '' ) {
        try {
            let formData = new FormData()
            formData.append('name', name.value)
            formData.append('description', description.value)
            formData.append('price', price.value)
            formData.append('image', image.value)
            formData.append('category_id', category_id.value)
            formData.append('_method', "PUT")


            let respone = await axios.post(urlAPI + '/' + route.params.id, formData, {
                headers: {
                    "Content-Type": "multipart/form-data"
                }
            })
            console.log(formData)

            if (respone.status === 200) {
                alert("Chỉnh sửa sản phẩm thành công")
                router.push('/admin/product')
            }

        } catch (error) {
            console.log('Call API lỗi');

        }
    }

}


const checkEmpty = (field) => {
    switch (field) {
        case 'name':
            {
                if (name.value == '') {
                    nameError.value = "Tên sản phẩm không được để trống"
                } else {
                    nameError.value = ""
                }
                break;
            }


        case 'description':
            {
                if (description.value == '') {
                    descriptionError.value = "Mô tả sản phẩm không được để trống"
                } else {
                    descriptionError.value = ""
                }
            }


            break

        case 'price': {
            if (price.value == '') {
                priceError.value = "Giá sản phẩm không được để trống"
            } else {
                priceError.value = ""
            }

            break;
        }
        case 'category_id': {
            if (category_id.value == '') {
                categoryError.value = "Vui lòng chọn danh mục"
            } else {
                categoryError.value = ""
            }

            break;
        }

        default: {
            break;

        }

    }
}


</script>

<template>
    <div class="p-4" style="min-height: 800px;">
        <h1>Trang sử<a href="http://" target="_blank" rel="noopener noreferrer"></a> sản phẩm</h1>
        <form action="" @submit.prevent="handleSubmit">
            <div class="mb-3">
                <label for="name">Name</label>
                <input type="text" class="form-control" id="name" v-model="name" @keyup="checkEmpty('name')">
                <p v-if="nameError != ''" class="text-danger">{{ nameError }}</p>
            </div>
            <div class="mb-3">
                <label for="category">Category</label>
                <select id="category" class="form-control" v-model="category_id">
                    <option value="" hidden>--Chọn danh mục</option>
                    <option :value="category.id" v-for="(category,index) in listCategory" :key="index">{{ category.name }}</option>
                </select>
                <p v-if="categoryError != ''" class="text-danger">{{ categoryError }}</p>

            </div>
            <div class="mb-3">
                <label for="description">Description</label>
                <input type="text" class="form-control" id="description" v-model="description"
                    @keyup="checkEmpty('description')">
                <p v-if="descriptionError != ''" class="text-danger">{{ descriptionError }}</p>

            </div>
            <div class="mb-3">
                <label for="price">Price</label>
                <input type="text" class="form-control" id="price" v-model="price" @keyup="checkEmpty('price')">
                <p v-if="priceError != ''" class="text-danger">{{ priceError }}</p>

            </div>
            <div class="mb-3">
                <label for="imgae">Image</label>
                <div>
                    <img v-if="imageOldUrl != null" :src="urlImage + imageOldUrl" alt="" width="50">
                    <span v-else class="badge text-bg-warning">Không có ảnh</span>
                </div>
                <input type="file" class="form-control" id="image" accept="image/*" @change="handleChange">
                <p v-if="imageError != ''" class="text-danger">{{ imageError }}</p>
            </div>
            <button class="btn btn-success">Chỉnh sửa</button>

        </form>

    </div>
</template>