<template>
    <footer class="flex flex-col w-full">
        <div class="flex flex-row items-center justify-center w-3/4 mx-auto my-10">
            <div class="group flex flex-col px-5 py-10 w-1/4 items-center justify-center gap-5 border-gray-50 border-2 hover:bg-slate-50 transition-all duration-200">
                <div class="">
                    <img src="/assets/image/footer/vice_item_1_thumb.webp" class="transition-all duration-500 transform group-hover:opacity-60" alt="">
                </div>
                <div class="flex flex-col gap-4">
                    <div class="text-center text-base transition-all duration-500 transform group-hover:scale-110">Giao Hàng & Lắp Đặt</div>
                    <div class="text-center text-gray-400 text-sm">Miến Phí</div>
                </div>
            </div>
            <div class="group flex flex-col px-5 py-10 w-1/4 items-center justify-center gap-5 border-gray-50 border-2 hover:bg-slate-50 transition-all duration-200">
                <div>
                    <img src="/assets/image/footer/vice_item_2_thumb.webp" class="transition-all duration-500 transform group-hover:opacity-60 " alt="">
                </div>
                <div class="flex flex-col gap-4">
                    <div class="text-center text-base transition-all duration-500 transform group-hover:scale-110">Đổi Trả 1 - 1</div>
                    <div class="text-center text-gray-400 text-sm">Miến Phí</div>
                </div>
            </div>
            <div class="group flex flex-col px-5 py-10 w-1/4 items-center justify-center gap-5 border-gray-50 border-2 hover:bg-slate-50 transition-all duration-200">
                <div>
                    <img src="/assets/image/footer/vice_item_3_thumb.webp" class="transition-all duration-500 transform group-hover:opacity-60" alt="">
                </div>
                <div class="flex flex-col gap-4">
                    <div class="text-center text-base transition-all duration-500 transform group-hover:scale-110">Bảo Hành 2 Năm</div>
                    <div class="text-center text-gray-400 text-sm">Miến Phí</div>
                </div>
            </div>
            <div class="group flex flex-col px-5 py-10 w-1/4 items-center justify-center gap-5 border-gray-50 border-2 hover:bg-slate-50 transition-all duration-200">
                <div>
                    <img src="/assets/image/footer/vice_item_4_thumb.webp" class="transition-all duration-500 transform group-hover:opacity-60" alt="">
                </div>
                <div class="flex flex-col gap-4">
                    <div class="text-center text-base transition-all duration-500 transform group-hover:scale-110">Tư Vấn Thiết Kế</div>
                    <div class="text-center text-gray-400 text-sm">Miến Phí</div>
                </div>
            </div>
        </div>
        <div class="container mx-auto md:px-40 grid md:grid-cols-2 lg:grid-cols-4  gap-5 py-2 px-10  bg-gray-100 dark:bg-black dark:text-white border">
            <div class="col-span-1 flex flex-col gap-2">
                <h1 class="uppercase font-semibold">Nội Thất MOHO</h1>
                <p class="text-gray-600 dark:text-white">Nội Thất MOHO là thương hiệu với gần 40 năm kinh nghiệm trong việc sản xuất và xuất khẩu nội thất đạt chuẩn quốc tế.</p>
            </div>
            <div class="col-span-1 flex flex-col gap-2">
                <h1 class="uppercase font-semibold">Dịch Vụ</h1>
                <ul class="text-gray-600 dark:text-white">
                    <li><a href="">Chính Sách Bán Hàng</a></li>
                    <li><a href="">Chính Sách Giao Hàng & Lắp Đặt</a></li>
                    <li><a href="">Chính Sách Đổi Trả</a></li>
                    <li><a href="">Chính Sách Bảo Hành & Bảo Trì</a></li>
                    <li><a href="">Khách Hàng Thân Thiết – MOHOmie</a></li>
                    <li><a href="">Chính Sách Đối Tác Bán Hàng</a></li>
                    <li><a href="">Chính Sách MOHO Home Planner</a></li>
                </ul>
            </div>
            <div class="col-span-1 flex flex-col gap-2">
                <h1 class="uppercase font-semibold">Thông tin liên hệ</h1>
                <div class="flex gap-2 text-gray-600 dark:text-white">
                    <VueIcon type="mdi" :path="mdiMapMarker" width="100"/>
                    <div class="">
                        <p> (Nằm trong khuôn viên công ty SAVIMEX phía sau bến xe buýt Hiệp Thành) | Hotline: 0971 141 140</p>
                        <p>Experience Store 1: S05.03-S18 phân khu The Rainbow | Vinhomes Grand Park, TP. | Hotline: </p>
                    </div>
                </div>
                <div class="flex gap-2 text-gray-600 dark:text-white">
                    <VueIcon type="mdi" :path="mdiPhoneClassic"/>
                    <div>
                        <p>097 114 1140 (Hotline/Zalo)</p>
                        <p>0902 415 359 (Đội Giao Hàng)</p>
                    </div>
                </div>
                <div class="flex gap-2 text-gray-600 dark:text-white">
                    <VueIcon type="mdi" :path="mdiEmail"/>
                    <p>cskh@moho.com.vn</p>
                </div>
            </div>
            <div class="col-span-1 flex flex-col gap-2">
                <h1 class="uppercase font-semibold">Fanpage</h1>
                <div></div>
            </div>
        </div>
        <div class="flex flex-col gap-2 items-center py-2">
            <div class="flex  gap-5">
                <VueIcon type="mdi" :path="mdiFacebook" size="30" class="text-blue-500 "/>
                <VueIcon type="mdi" :path="mdiYoutube" size="30" class="text-red-500"/>
                <VueIcon type="mdi" :path="mdiInstagram" size="30" class="bg-gradient-to-r from-purple-400 via-pink-500 to-red-500 rounded-full text-white"/>
                <VueIcon type="mdi" :path="mdiMusicCircle" size="30" class="text-black dark:text-white"/>
            </div>
            <p class="text-gray-500">Chỉ đường đến showroom trên Google Maps</p>
        </div>
        <p class="py-2 bg-black text-white text-center dark:text-black dark:bg-white">Copyright &copy; 2024 Nội Thất MOHO.</p>
    </footer>
</template>
<script>
import {mdiMapMarker,mdiPhoneClassic,mdiEmail,mdiFacebook,mdiYoutube,mdiInstagram,mdiMusicCircle} from "@mdi/js"
export default {
    name:"FooterView",
    data(){
        return{
            mdiMapMarker,mdiPhoneClassic,mdiEmail,mdiFacebook,mdiYoutube,mdiInstagram,mdiMusicCircle
        }
    }
}
</script>