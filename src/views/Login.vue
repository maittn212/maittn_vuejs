<script setup>
import 'bootstrap/dist/css/bootstrap.min.css'
import 'bootstrap/dist/js/bootstrap.bundle.min.js'
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';
import { RouterLink } from 'vue-router';

let url = 'http://localhost/vuejs-server/api.php/login'

const router = useRouter()
const email = ref("")
const password = ref("")
const emailError = ref("")
const passwordError = ref("")

const handleSubmit = async () => {
    try {
        let data = {
            email: email.value,
            password: password.value
        }
        let respone = await axios.post(url, data)
        let message = respone.data

        if (message.status == true) {
            localStorage.removeItem('userLogin')
            let userLogin = {
                id: message.data.id,
                name: message.data.name,
                email: message.data.email,
                role: message.data.role

            }

            localStorage.setItem('userLogin', JSON.stringify(userLogin));
            if (message.data.role == 'user') {
                router.push('/')
            } else if (message.data.role == 'admin') {
                router.push('/admin')

            }
        } else {
            alert('Tài khoản hoặc mật khẩu không đúng')

        }
    } catch (error) {
        console.log(error);
    }
}

const checkValidate = (field) => {
    switch (field) {
        case 'email':
            if (email.value == '') {
                emailError.value = "Email không được để trống"
            }else{
                emailError.value = ''
            }
            break
        case 'password':
            if (password.value == '') {
                passwordError.value = "Password không được để trống"
            }else{
                passwordError.value = ''
            }
            break
        default:
            break
    }

}

</script>

<template>
    <form @submit.prevent="handleSubmit">
        <h1>Đăng nhập</h1>
        <div class="mb-4">
            <label for="email">Email</label>
            <input type="text" placeholder="Email" id="email" class="form-control" v-model="email"
                @keyup="checkValidate('email')">
            <span class="text-danger" v-if="emailError !== ''">{{ emailError }}</span>
        </div>
        <div class="mb-4">
            <label for="password">Password</label>
            <input type="password" placeholder="Passsword" id="password" class="form-control" v-model="password"
                @keyup="checkValidate('password')">
            <span class="text-danger" v-if="passwordError !== ''">{{ passwordError }}</span>
        </div>
        <span class="register">Bạn chưa có tài khoản? <RouterLink to="/register"> Đăng ký</RouterLink></span>
        <button class="btn btn-primary">Đăng nhập</button>
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