<script setup>
import '@/assets/client.css'

import Header from '@/components/User/Header.vue'
import Navbar from '@/components/User/Navbar.vue'
import Footer from '@/components/User/Footer.vue'

import { RouterView } from 'vue-router';
</script>

<template>
    <div class="container-fluid p-0">
        <div class="row">
            <div class="col-12">
                <!-- Header  -->
                <Header />
                <!-- Header  -->
            </div>
        </div>
        <div class="row main-website">
            <div class="col-12">
                <!-- Navbar  -->
                <Navbar />
                <!-- Navbar  -->
            </div>
        </div>

        <RouterView />

        <div class="row">
            <div class="col-12">
                <!-- Footer  -->
                <Footer />
                <!-- Footer  -->

            </div>
        </div>

    </div>
</template>
<style scoped lang="scss">
.img-header {
    height: 400px;
    object-fit: cover;
}
</style>