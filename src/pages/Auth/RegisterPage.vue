<template lang="">
    <div class="h-[100vh] flex">
        <div class="w-[40%] flex items-center justify-center max-md:hidden">
            <img class="w-[100%] object-cover" src="../../assets/images/undraw_Updated_resume_re_7r9j.png" alt="img" >
        </div>
        <div class="relative flex items-center justify-center w-[60%] max-md:w-[100%]">
            <div class="absolute top-[70px] left-[150px]">
                <p class="text-[16px] font-medium text-gray-500">Welcome to</p>
                <h1 class="font-bold text-[25px] text-green-500">Juong Job.</h1>
            </div>
            <div class="w-[300px] flex flex-col items-center">
                <h3 class="font-semibold text-[20px] mb-[30px]">Register</h3>
                <form action="" class="flex flex-col items-center gap-5 w-[100%]" @submit.prevent="handleRegister">
                    <div class="flex flex-col gap-1 w-[100%] rounded">
                        <label for="" class="text-[14px] font-semibold">Full name</label>
                        <input type="text" v-model="userData.name" placeholder="Enter fullname" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                    </div>
                    <div class="flex flex-col gap-1 w-[100%] rounded">
                        <label for="" class="text-[14px] font-semibold">Phone number</label>
                        <input type="text" v-model="userData.phone" placeholder="Enter phone number" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                    </div>
                    <div class="flex flex-col gap-1 w-[100%] rounded">
                        <label for="" class="text-[14px] font-semibold">Email</label>
                        <input type="text" v-model="userData.email" placeholder="enter email" class="outline-none text-[13px] px-[10px] py-[7px] border-[1px]">
                    </div>
                    <div class="flex flex-col gap-1 w-[100%] rounded">
                        <label for="" class="text-[14px] font-semibold">Password</label>
                        <div class="border-[1px]">
                            <input :type="isShow ? 'text' : 'password'" v-model="userData.password" placeholder="enter password" class="outline-none text-[13px] px-[10px] py-[7px] w-[90%]">
                            <EyeOutlined v-if="!isShow" :style="{cursor: 'pointer'}" @click="handleToggleShowPassword"/>
                            <EyeInvisibleOutlined v-if="isShow" :style="{cursor: 'pointer'}" @click="handleToggleShowPassword"/>
                        </div>
                    </div>
                    <button :type="submit" class="text-[16px] font-medium px-[40px] py-[10px] text-white bg-green-500 rounded " >Register</button>
                </form>
                <p class="font-normal text-[13px] mt-5">I already have an account?<span class="text-green-500 cursor-pointer" @click="goLoginPage">Login</span></p>
            </div>
        </div>
        <Loading v-if="userStore.isLoading"/>
    </div>
</template>
<script setup>
    import {EyeOutlined, EyeInvisibleOutlined} from "@ant-design/icons-vue"
    import {ref} from "vue"
    import {useRouter} from "vue-router"
    import {useUserStore} from "../../stores/userStore"
    import { useToast } from "vue-toastification";
    import Loading from "../../components/Loading.vue";
    const toast = useToast()
    const userStore = useUserStore()
    const router = useRouter()
    const isShow = ref(false)
    const userData = ref({
        name: "",
        phone: "",
        email: "",
        password: "",
    })
   

    const handleToggleShowPassword = () => {
        isShow.value = !isShow.value
    }

    const goLoginPage = () => {
        router.push("/auth-layout")
    }

    const handleRegister = async () => {
        if (!userData.value.name || !userData.value.phone || !userData.value.email || !userData.value.password) {
            toast.warning("Please enter complete information!");
        } else if (!isValidEmail(userData.value.email)) {
            toast.warning("Invalid email!");
        } else if (userData.value.password.length < 6) {
            toast.warning("Password must be at least 6 characters!");
        } else if (userData.value.phone.length < 10 || userData.value.phone.length > 10) {
            toast.warning("Phone number must be 10 digits");
        } else {
            const data = await userStore.actRegisterUser(userData.value)
            if(data) {
                router.push("/auth-layout")
            }
        }
    }

    const isValidEmail = (email) => {
    // Sử dụng biểu thức chính quy để kiểm tra tính hợp lệ của địa chỉ email.
        const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return emailPattern.test(email);
    };

</script>
<style >

</style>