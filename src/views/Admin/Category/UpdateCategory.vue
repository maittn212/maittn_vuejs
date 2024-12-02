<script setup>
import { useRoute, useRouter } from 'vue-router';
import { ref, onMounted } from 'vue';
import axios from 'axios';
let url = 'http://localhost/vuejs-server/api.php/categories'
const route = useRoute()
const router = useRouter()
// console.log(route.params.idCategory)

const name = ref("")
const description = ref("")

onMounted(async () => {
    let respone = await axios.get(url + "/" + route.params.idCategory)
    if (respone) {
        name.value = respone.data.name
        description.value = respone.data.description
    }
})

const handleSubmit = async () => {
    checkValidate()
    if (nameError.value == '' && descriptionError.value == '') {
        let formData = new FormData();
        formData.append("_method", "PUT")
        formData.append("name", name.value)
        formData.append("description", description.value)

        let respone = await axios.post(
            url + "/" + route.params.idCategory,
            formData, {
            headers: {
                "Content-Type": "multipart/form-data",
            },
        }
        )
        if (respone) {
            alert(respone.data.message);
            router.push("/admin/category")
        }

    }

}


const nameError = ref("")
const descriptionError = ref("")
const statusError = ref(false)

const checkValidate = async () => {
    if (name.value == '') {
        nameError.value = 'Tên danh mục không được để trống'
    } else {
        nameError.value = '';
    }
    if (description.value == '') {
        descriptionError.value = 'Mô tả danh mục không được để trống'
        statusError.value = true
    } else {
        descriptionError.value = '';

    }

}
</script>
<template>
    <div class="p-4" style="min-height: 800px;">
        <h1>Trang sửa danh mục</h1>
        <form @submit.prevent="handleSubmit">
            <div class="mb-3">
                <label for="name">Name</label>
                <input type="text" id="name" class="form-control" placeholder="Tên danh mục" v-model="name">
                <span class="text-danger" v-if="nameError !== ''">{{ nameError }}</span>
            </div>
            <div class="mb-3">
                <label for="name">Description</label>
                <input type="text" id="description" class="form-control" placeholder="Mô tả danh mục"
                    v-model="description">
                <span class="text-danger" v-if="descriptionError !== ''">{{ descriptionError }}</span>

            </div>
            <button class="btn btn-success">Sửa</button>
        </form>
    </div>
</template>