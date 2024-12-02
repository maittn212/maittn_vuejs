<script setup>
import 'bootstrap/dist/css/bootstrap.min.css'
import 'bootstrap/dist/js/bootstrap.bundle.min.js'
import { ref } from 'vue';
import { RouterLink,useRouter } from 'vue-router';
import axios from 'axios';

let url = 'http://localhost/vuejs-server/api.php/register'

const router = useRouter()
const name = ref('')
const email = ref('')
const password = ref('')
const nameError = ref('')
const emailError = ref('')
const passwordError = ref('')

const handleSubmit = async()=>{
  try {
    let data = {
        name: name.value,
        email: email.value,
        password: password.value,
    }
    let respone = await axios.post(url,data)
    if(respone.data.message){
        alert('Đăng ký thành công')
        router.push('/login')
    }
  } catch (error) {
    console.log(error);
    
  }
}

const handleChange = async (field) => {
    switch (field) {
        case 'name':
            if (name.value == '') {
                nameError.value = 'Name không được để trống'
            } else {
                nameError.value = ''
            }
            break
        case 'email':
            if (email.value == '') {
                emailError.value = 'Email không được để trống'
            } else {
                emailError.value = ''
            }
            break;
        case 'password':
            if (password.value == '') {
                passwordError.value = 'Password không được để trống'
            } else {
                passwordError.value = ''
            }
            break;
        default:
            break

    }
}

</script>
<template>
    <form @submit.prevent="handleSubmit">
        <h1>Đăng ký</h1>
        <div class="mb-4">
            <label for="name">Name</label>
            <input type="text" placeholder="Name" class="form-control" v-model="name" @keyup="handleChange('name')">
            <span class="text-danger" v-if="nameError != ''">{{ nameError }}</span>
        </div>
        <div class="mb-4">
            <label for="email">Email</label>
            <input type="text" placeholder="Email" class="form-control" v-model="email" @keyup="handleChange('email')">
            <span class="text-danger" v-if="emailError != ''">{{ emailError }}</span>

        </div>
        <div class="mb-4">
            <label for="name">Passsword</label>
            <input type="text" placeholder="Password" class="form-control" v-model="password" @keyup="handleChange('password')">
            <span class="text-danger" v-if="passwordError != ''">{{ passwordError }}</span>

        </div>
        <span class="register">Bạn đã có tài khoản? <RouterLink to="/login"> Đăng nhập</RouterLink></span>

        <button class="btn btn-primary">Đăng ký</button>
    </form>
</template>
<style scoped lang="scss">
form {
    width: 60%;
    margin: auto;
    margin-top: 50px;

    .register {
        display: flex;
        margin-bottom: 10px;
    }
}
</style>