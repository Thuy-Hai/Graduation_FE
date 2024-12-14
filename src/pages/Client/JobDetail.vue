<template lang="">
    <div class="max-w-[1300px] mx-auto mt-[100px] flex gap-5">
        <div class="rounded w-[30%] h-max flex flex-col gap-5 bg-green-100 py-[40px] px-[20px] max-md:hidden">
            <div>
                <img v-if="handleCheckLinkImage(postData?.business?.avatar)" :src="postData?.business?.avatar" alt="logo" class="h-[50px] rounded object-cover">
                <img v-else :src="`${IMAGE_URL}${postData?.business?.avatar}`" alt="logo" class="h-[50px] rounded object-cover">
                <p class="mt-2 font-medium">{{postData?.business?.name}}</p>
            </div>
            <p class="text-gray-500">
                {{postData?.business?.career}}
            </p>
            <div>
                <p class="">Number of posts:</p>
                <p class="font-semibold">{{quantityJob}} Jobs</p>
            </div>
            <div>
                <p class="">Company size:</p>
                <p class="font-semibold">{{postData?.business?.size}}</p>
            </div>
            <div>
                <p class="">Email:</p>
                <p class="font-semibold">{{postData?.business?.email}}</p>
            </div>
            <div>
                <p class="">Website:</p>
                <a :href="postData?.business?.website" target="_blank" class="font-semibold">{{postData?.business?.website}}</a>
            </div>
            <div>
                <p class="">Company address:</p>
                <p class="font-semibold">{{postData?.business?.location}}</p>
            </div>
            <button class="bg-green-500 text-white text-[14px] font-semibold px-[15px] py-[10px] rounded" @click="goDetailCompany(postData?.business?.id)">more</button>
        </div>
        <div class="w-[70%] max-md:w-full max-sm:px-[20px]">
            <div class="shadow-lg flex flex-col gap-5 px-[30px] py-[30px] rounded">
                <div class="flex items-center justify-between border-b pb-[20px]">
                    <div class="flex flex-col" @click="goDetailCompany(postData?.business?.id)">
                        <img v-if="handleCheckLinkImage(postData?.business?.avatar)" :src="postData?.business?.avatar" alt="logo" class="h-[50px] rounded object-cover">
                        <img v-else :src="`${IMAGE_URL}${postData?.business?.avatar}`" alt="logo" class="h-[50px] rounded object-cover">
                        <p class="mt-2 font-medium">{{postData?.business?.name}}</p>
                    </div>
                    <div class="flex flex-col">
                        <p class="font-medium text-[18px]">{{formatSalary(postData?.salary)}}<span class="text-[14px] font-normal">/month</span></p>
                        <button class="px-[10px] py-[5px] rounded bg-green-200 text-green-600" @click="handleToggleApplyPopUp">Apply</button>
                    </div>
                </div>
                <div class="flex items-center justify-between">
                    <div>
                        <p class="text-[30px] font-semibold mb-2">{{postData?.position}}</p>
                        <div class="flex gap-5 items-center flex-wrap">
                            <div class="flex items-center gap-1">
                                <EnvironmentOutlined :style="{fontSize: '14px', color: '#9BA4B5'}"/>
                                <span>{{postData?.business?.location}}</span>
                            </div>
                            <div class="flex items-center gap-1">
                                <ClockCircleOutlined :style="{fontSize: '14px', color: '#9BA4B5'}"/>
                                <span v-for="(type, index) in postData?.type">{{type}}/</span>
                            </div>
                            <div class="flex items-center gap-1">
                                <CheckCircleOutlined :style="{fontSize: '14px', color: '#9BA4B5'}"/>
                                <span v-for="(level, index) in postData?.level">{{level}}/</span>
                            </div>
                            <div class="flex items-center gap-1">
                                <UsergroupAddOutlined :style="{fontSize: '14px', color: '#9BA4B5'}"/>
                                <span>Number of applications:</span>
                                <span >{{postData?.quantity}}</span>
                            </div>
                            <div class="flex items-center gap-1">
                                <EyeOutlined :style="{fontSize: '14px', color: '#9BA4B5'}"/>
                                <span>View:</span>
                                <span >{{postData?.view_count}}</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="mt-[50px] flex flex-col gap-5">
                <div>
                    <h2 class="text-[24px] font-medium">Job Description</h2>
                    <p class="mt-3 text-gray-500" v-html="postData?.content">

                    </p>
                </div>
                <div>
                    <h2 class="text-[24px] font-medium">requirement</h2>
                    <p class="mt-3 text-gray-500" v-html="postData?.requirement">

                    </p>
                </div>
                <div>
                    <h2 class="text-[24px] font-medium">benefits</h2>
                    <p class="mt-3 text-gray-500"  v-html="postData?.benefits">

                    </p>
                </div>
                <div>
                    <h2 class="text-[24px] font-medium">Skills</h2>
                    <p class="mt-3 text-gray-500">
                        Lorem ipsum dolor sit, amet consectetur adipisicing elit. Dolorem nostrum dolores accusamus a vero consequuntur dolorum, autem repellat quaerat architecto officia quos animi? Ipsum aperiam delectus consectetur labore, nesciunt voluptatem!
                    </p>
                    <div class="mt-5 flex flex-wrap gap-3 px-[20px]">
                        <div class="px-[15px] py-[5px] bg-green-100 text-green-600 font-semibold rounded" v-for="(skill, index) in postData?.skill" :key="index" >
                            {{skill}}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <Loading  v-if="postStore.isLoading"/>
    <ApplyPopup v-if="isApply" :toggle="handleToggleApplyPopUp" :user="userStore.myUser" :job="postData"/>
</template>
<script setup>
    import {EnvironmentOutlined, EyeOutlined, ClockCircleOutlined, CheckCircleOutlined, UsergroupAddOutlined} from "@ant-design/icons-vue"
    import { useRoute, useRouter } from "vue-router";
    import { usePostStore } from "../../stores/postStore";
    import { computed, onMounted, ref } from "vue";
    import ApplyPopup from "../../components/ApplyPopup.vue"
    import { IMAGE_URL } from "../../constants/url";
    import Loading from "../../components/Loading.vue";
import { useUserStore } from "../../stores/userStore";
import { useToast } from "vue-toastification";
import { fetchPostByIdBusiness } from "../../api/postApi";
    const route = useRoute()
    const router = useRouter()
    const postStore = usePostStore()
    const idPost = route.params.id
    const isApply = ref(false)
    const userStore = useUserStore()
    const toast = useToast()
    const quantityJob = ref(0)
    const goDetailCompany = (id) => {
        router.push(`/company/${id}`)
    }

    const handleCheckLinkImage = (url) => {
        return url?.startsWith("http");
    }
    const handleToggleApplyPopUp = () => {
        if(userStore.isLogged === true && userStore.accessToken) {
            isApply.value = !isApply.value
        }else {
            toast.warning("Please login before applying")
        }
    }

    const handleGetPostById = async(idPost) => {
       await postStore.actGetPostById(idPost)
    }

    onMounted(() => {
        handleGetPostById(idPost)
        handleCalJobByBusinessId(postStore.post.business?.id)
    })
    
    const handleCalJobByBusinessId = async(id) => {
        const data = await fetchPostByIdBusiness(id)
        if(data) {
            quantityJob.value = data?.data?.length
        }
    }

    const postData = computed(() => {
        return postStore.post
    })

    
const formatSalary = (salary) => {
    if (!salary) return "0";

    let salaryString = salary.toString();
    salaryString = salaryString.replace(/[^\d.]/g, "");
    if (salaryString.indexOf('.') !== -1) {
        salaryString = salaryString.replace(/\.(?=\d*$)/, '');
    }

    let salaryValue = parseInt(salaryString.replace(/\./g, ''), 10) || 0;

    let formattedSalary = salaryValue.toLocaleString('de-DE');

    return formattedSalary;
};


</script>
<style lang="">
    
</style>