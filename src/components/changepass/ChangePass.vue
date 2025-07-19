<template>
    <div class="w-full dark:bg-black dark:text-white">
        <h1 class="text-xl font-bold">Đổi mật khẩu<div class="border-b-4 border-yellow-500 w-14"></div></h1>
        <div class="py-2 flex flex-col gap-2">
            <div class="flex flex-col md:w-1/2">
                <label for="">Mật khẩu cũ *</label>
                <input type="password" v-model="OldPassword" class="border outline-none dark:bg-black dark:text-white p-2">
            </div>
            <div class="flex flex-col md:w-1/2">
                <label for="">Mật khẩu mới *</label>
                <input type="password" v-model="NewPassword" class="border outline-none dark:bg-black dark:text-white p-2">
            </div>
            <div class="flex flex-col md:w-1/2">
                <label for="">Xác nhận lại mật khẩu *</label>
                <input type="password" v-model="ConfirmNewPassword" class="border outline-none dark:bg-black dark:text-white p-2">
            </div>
            <button @click="Update" class="p-2 bg-yellow-600 text-white w-1/2 rounded-md border border-yellow-600 hover:text-yellow-600 hover:bg-white">Đặt lại mật khẩu</button>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: "ChangePassView",
    data() {
        return {
            OldPassword: "",
            NewPassword: "",
            ConfirmNewPassword: ""
        };
    },
    methods: {
        async Update() {
            try {
                await axios.post(`/User/changePassword`, {
                    OldPassword: this.OldPassword,
                    NewPassword: this.NewPassword,
                    ConfirmNewPassword: this.ConfirmNewPassword
                }, {
                    headers: {
                        "Authorization": `Bearer ${this.$store.state.token}`
                    }
                });
                this.$toast.success(`Cập nhật thành công`, {
                    position: 'top-right',
                    timeout: 5000,
                });
            } catch (err) {
                if (err.response.status === 401) {
                    this.$toast.error("Phiên đăng nhập hết hạn. Vui lòng đăng nhập lại.", {
                        position: 'top-right',
                        timeout: 5000,
                    });
                    this.$router.push('/login');
                } else {
                    this.$toast.error(`${err.response.data}`, {
                        position: 'top-right',
                        timeout: 5000,
                    });
                }
            }
        }
    }
};
</script>