<template lang="">
    <div class="max-w-[1300px] flex justify-center mx-auto mt-[150px] ">
        <div class="w-[700px] bg-white shadow rounded-xl p-[20px]">
            <h1 class="font-semibold text-[20px] mb-[20px]">Change login password</h1>
            <form action=""  class="flex flex-col gap-5">
                <div class="flex flex-col gap-2">
                    <label for="" class="text-[14px] font-medium">Email</label>
                    <input type="email" disabled class="outline-none px-[10px] py-[5px] rounded border">
                </div>
                <div class="flex flex-col gap-2">
                    <label for="" class="text-[14px] font-medium">Current Password</label>
                    <input type="password" class="outline-none px-[10px] py-[5px] rounded border">
                </div>
                <div class="flex flex-col gap-2">
                    <label for="" class="text-[14px] font-medium">New Password</label>
                    <input type="password" class="outline-none px-[10px] py-[5px] rounded border">
                </div>
                <div class="flex flex-col gap-2">
                    <label for="" class="text-[14px] font-medium">Re-enter new password</label>
                    <input type="password" class="outline-none px-[10px] py-[5px] rounded border">
                </div>
                <button class="px-[10px] py-[5px] text-white bg-green-500 font-semibold rounded">Save</button>
            </form>
        </div>
    </div>
</template>
<script>
export default {
    
}
</script>
<style lang="">
    
</style>