<template lang="">
    <div class="h-[100vh] flex">
        <div class="w-[40%] flex items-center justify-center max-md:hidden">
            <img class="w-[100%] object-cover" src="../../assets/images/undraw_Business_deal_re_up4u.png" alt="img" >
        </div>
        <div class="relative flex items-center justify-center w-[60%] max-md:w-[100%]">
            <div class="absolute top-[70px] left-[150px]">
                <p class="text-[16px] font-medium text-gray-500">Welcome to</p>
                <h1 class="font-bold text-[25px] text-green-500">Juong Job.</h1>
            </div>
            <div class="w-[300px] flex flex-col items-center">
                <h3 class="font-semibold text-[20px] mb-[30px]">Business registration</h3>
                <form action="" class="flex flex-col items-center gap-5 w-max" @submit.prevent="handleRegister">
                    <div class="flex gap-3">
                        <div class="flex flex-col gap-1 w-[100%] rounded">
                            <label for="" class="text-[14px] font-semibold">Name Company</label>
                            <input type="text" v-model="businessData.name" placeholder="Enter name company" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                        </div>
                        <div class="flex flex-col gap-1 w-[100%] rounded">
                            <label for="" class="text-[14px] font-semibold">Phone number</label>
                            <input type="text" v-model="businessData.phone" placeholder="Enter phone number" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                        </div>
                    </div>
                   
                    <div class="flex flex-col gap-1 w-[100%] rounded">
                        <label for="" class="text-[14px] font-semibold">Description</label>
                        <input type="text" v-model="businessData.career" placeholder="enter Description" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                    </div>
                    <div class="flex items-center gap-3">
                        <div class="flex flex-col gap-1 w-[100%] rounded">
                            <label for="" class="text-[14px] font-semibold">Email</label>
                            <input type="email" v-model="businessData.email" placeholder="enter email" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                        </div>
                        <div class="flex flex-col gap-1 w-[100%] rounded">
                            <label for="" class="text-[14px] font-semibold">Address</label>
                            <input type="text" v-model="businessData.location" placeholder="Enter company Address" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                        </div>
                    </div>
                    <div class="flex items-center gap-3">
                        <div class="flex flex-col gap-1 w-[100%] rounded">
                            <label for="" class="text-[14px] font-semibold">Website</label>
                            <input type="text" v-model="businessData.website" placeholder="enter link website" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                        </div>
                        <div class="flex flex-col gap-1 w-[100%] rounded">
                            <label for="" class="text-[14px] font-semibold">Number of employees</label>
                            <input type="text" v-model="businessData.size" placeholder="Enter Number of employees" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                        </div>
                    </div>
                    <div class="flex flex-col gap-1 w-[100%] rounded">
                        <label for="" class="text-[14px] font-semibold">Password</label>
                        <input type="password" v-model="businessData.password" placeholder="enter password" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                    </div>
                    <button :type="submit" class="text-[16px] font-medium px-[40px] py-[10px] text-white bg-green-500 rounded ">Đăng ký</button>
                </form>
                <p class="font-normal text-[13px] mt-5">I already have an account?<span class="text-green-500 cursor-pointer" @click="goLoginBusinessPage">Đăng nhập</span></p>
            </div>
        </div>
    </div>
    <Loading v-if="businessStore.isLoading"/>
</template>
<script setup>
import { ref } from 'vue';
import {useBusinessStore} from "../../stores/businessStore"
import { useRouter } from 'vue-router';
import Loading from '../../components/Loading.vue';
import { useToast } from 'vue-toastification';
    const businessStore = useBusinessStore()
    const router = useRouter()
    const toast = useToast()
    const businessData = ref({
        name: "",
        email: "",
        password: "",
        phone: "",
        location: "",
        website: "",
        career: "",
        size: ""
    })

    const handleRegister = async() => {
        if (!businessData.value.name || !businessData.value.phone || !businessData.value.email || !businessData.value.password || !businessData.value.location || !businessData.value.website || !businessData.value.career || !businessData.value.size) {
            toast.warning("Please enter complete information!");
        } else if (!isValidEmail(businessData.value.email)) {
            toast.warning("Invalid email!");
        } else if (businessData.value.password.length < 6) {
            toast.warning("Password must contain at least 6 characters!");
        } else if (businessData.value.phone.length < 10 || businessData.value.phone.length > 10) {
            toast.warning("Phone number must consist of 10 digits");
        } else {
            const data = await businessStore.actRegisterBusiness(businessData.value)
            if(data) {
                router.push("/auth-layout/login-business")
            }
        }
    }

    const isValidEmail = (email) => {
    // Sử dụng biểu thức chính quy để kiểm tra tính hợp lệ của địa chỉ email.
        const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return emailPattern.test(email);
    };

    const goLoginBusinessPage = () => {
        router.push("/auth-layout/login-business")
    }
</script>
<style >

</style>