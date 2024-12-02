<script setup>
import { onMounted, ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import { useRoute } from 'vue-router';

const router = useRouter()
const route = useRoute()
let url = 'http://localhost/vuejs-server/api.php/users'
const name = ref('')
const email = ref('')
const role = ref('')
const note = ref('')
const password = ref('')
const oldPassword = ref('******')


const nameError = ref('');
const emailError = ref('');
const roleError = ref('');
const passwordError = ref('');

onMounted(async ()=>{
    let respone = await axios.get(url + '/' + route.params.id)
    if(respone){
       name.value = respone.data.name
       email.value = respone.data.email
       role.value = respone.data.role
       note.value = respone.data.note
       oldPassword.value = respone.data.password
    }
})

const handleSubmit = async () => {
    checkValidate()
    if (nameError.value == '' && emailError.value == '' && roleError.value == '' && passwordError.value == '') {
        let formData = new FormData()
        formData.append("_method", "PUT")
        formData.append('name', name.value)
        formData.append('email', email.value)
        formData.append('role', role.value)
        formData.append('note', note.value)

        if(password.value){
            formData.append('password', password.value)       
        }
        
        try {
            let respone = await axios.post(url +'/' + route.params.id, formData, {
                headers: {
                    "Content-Type": "multipart/form-data"
                }
            })
            if (respone) {
                alert('Người dùng đã được cập nhật')
                router.push('/admin/account')
            }

        } catch (error) {
            console.log('Loi');
        }
    }

}

const checkValidate = async () => {
    if (name.value == '') {
        nameError.value = 'Tên không được để trống'
    } else {
        nameError.value = ''
    }

    if (email.value == '') {
        emailError.value = 'Email không được để trống'
}else if (!email.value.includes('@') && !email.value.includes('.')) {
        emailError.value = 'Email phải chứa @ và .'
    }else {
        emailError.value = ''
    }
    // if (password.value == '') {
    //     passwordError.value= ''
    // } else {
    //     passwordError.value = ''
    // }

    if (role.value == '') {
        roleError.value = 'Role không được để trống'
    } else if (role.value !== 'admin' && role.value !== 'user' && role.value !== 'moderator') {
        roleError.value = 'Role phải là admin, user hoặc moderator'
    } else {
        roleError.value = ''
    }
}

</script>
<template>
    <div class="p-4" style="min-height: 800px;">
        <h1>Sửa tài khoản</h1>
        <form @submit.prevent="handleSubmit">
            <div class="mb-3">
                <label for="name">Name</label>
                <input type="text" class="form-control" v-model="name">
                <span class="text-danger" v-if="nameError !== ''">{{ nameError }}</span>
            </div>
            <div class="mb-3">
                <label for="email">Email</label>
                <input type="text" class="form-control" v-model="email">
                <span class="text-danger" v-if="emailError !== ''">{{ emailError }}</span>
            </div>
            <div class="mb-3">
                <label for="oldPassword">Old Password</label>
                <input type="text" class="form-control" v-model="oldPassword" disabled>
                <small class="form-text text-muted">Mật khẩu đã được thiết lập. Nhập mật khẩu mới nếu bạn muốn thay đổi.</small>
            </div>
            <div class="mb-3">
                <label for="email">New password</label>
                <input type="text" class="form-control" v-model="password">
                <span class="text-danger" v-if="passwordError !== ''">{{ passwordError }}</span>
            </div>

            <div class="mb-3">
                <label for="email">Role</label>
                <input type="text" class="form-control" v-model="role" placeholder="user, admin, moderator">
                <span class="text-danger" v-if="roleError !== ''">{{ roleError }}</span>
            </div>
            <div class="mb-3">
                <label for="email">Note</label>
                <input type="text" class="form-control" v-model="note">
            </div>
            <button class="btn btn-success">Sửa</button>
        </form>
    </div>
</template>