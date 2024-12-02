<script setup>
let url = 'http://localhost/vuejs-server/api.php/users'
import { ref,onMounted } from 'vue';
import axios from 'axios';
import { RouterLink } from 'vue-router';

const listAccount = ref([])
onMounted(async()=>{
    let respone = await axios.get(url)
    listAccount.value = respone.data
})

const handleDelete = async(id)=>{
    const check = confirm('Bạn có muốn xóa không?');
    if(check){
        let respone = await axios.delete(url + '/' + id)
        if(respone){
            alert(respone.data.message)
            let respone2 = await axios.get(url)
            listAccount.value = respone2.data
        }
    }
}

</script>
<template>
    <div class="p-4" style="min-height: 800px;">
        <RouterLink to="/admin/add-account" class="btn btn-primary">Thêm mới</RouterLink>
        <h1>Quản lý tài khoản</h1>
        <table class="table">
            <thead>
                <tr>
                    <th>STT</th>
                    <th>Name</th>
                    <th>Email</th>
                    <th>Role</th>
                    <th>Note</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(account,index) in listAccount" :key="index">
                    <td>{{ index+1 }}</td>
                    <td>{{ account.name }}</td>
                    <td>{{ account.email}}</td>
                    <td>{{ account.role }}</td>
                    <td>{{ account.note }}</td>
                    <td>
                        <RouterLink :to="`/admin/update-account/${account.id}`" class="btn btn-warning">Sửa</RouterLink>
                        <button class="btn btn-danger" @click="handleDelete(account.id)">Xóa</button>
                    </td>

                </tr>
            </tbody>
        </table>
    </div>
    
</template>