<template>
  <div class="z-[1000]">
    <nav
      v-if="!isSticky"
      class="navbar-normal container mx-auto md:px-40 flex flex-col border-b dark:bg-black transition-all duration-300 opacity-100 translate-y-0"
    >
      <div class="flex justify-between w-full dark:bg-black items-center gap-2 py-5">
        <!-- Menu di động -->
        <div
          ref="menuContainer"
          @click="openMenu"
          class="flex lg:hidden relative items-center cursor-pointer dark:bg-black dark:text-white"
        >
          <VueIcon type="mdi" :path="!menu ? mdiViewHeadline : mdiClose" size="40" />
          <ul
            id="menu"
            :class="menu ? 'flex' : 'hidden'"
            class="absolute flex-col font-bold bg-white dark:bg-black dark:text-white w-[50vw] z-10 top-14 -left-5 shadow-[0px_0px_2px_2px_rgba(0,0,0,0.3)]"
          >
            <li>
              <CategoryMenu
                :category="category"
                :isOpen="isMenuOpen"
                :onClick="toggleMenu"
                ulClass="px-2"
                liClass="flex flex-col py-1.5"
                subUlClass="px-4"
              />
            </li>
            <li class="flex items-center p-2 hover:bg-gray-200 hover:text-red-500">
              <router-link :to="{ name: 'about' }" class="flex">Dịch Vụ</router-link>
            </li>
            <li class="flex items-center p-2 hover:bg-gray-200 hover:text-red-500">
              <router-link :to="{ name: 'about' }" class="flex">Về chúng tôi</router-link>
            </li>
            <li class="flex items-center p-2 hover:bg-gray-200 hover:text-red-500">
              <router-link :to="{ name: 'contact' }" class="flex">Liên hệ</router-link>
            </li>
          </ul>
        </div>

        <!-- Logo -->
        <div class="col-span-2 justify-center">
          <router-link :to="{ name: 'home' }">
            <img src="/assets/image/logo.png" alt="" class="w-50 h-15 object-cover" />
          </router-link>
        </div>

        <!-- Ô tìm kiếm (Desktop) -->
        <div ref="searchContainer" class="w-80 hidden md:flex">
          <div
            class="flex items-center relative dark:bg-black dark:text-white border-2 p-1 border-gray-100 w-80 h-10 rounded focus-within:border-gray-400"
          >
            <input
              type="text"
              v-model="searchValue"
              placeholder="Tìm kiếm sản phẩm..."
              class="outline-none w-2/3 dark:bg-black dark:text-white rounded"
              @focus="showSuggestions = true"
            />
            <router-link
              :to="{ name: 'search', query: { q: searchValue } }"
              class="absolute right-0 bg-gray-700 dark:bg-white h-10 w-10 flex justify-center items-center rounded border-none"
            >
              <VueIcon type="mdi" :path="mdiMagnify" class="dark:text-black text-white" />
            </router-link>
            <div
              v-if="showSuggestions"
              class="absolute top-10 bg-white dark:bg-black dark:text-white max-h-80 overflow-x-auto w-full z-10 shadow-md rounded-sm"
            >
              <div class="p-1 bg-gray-200 dark:text-black">Sản phẩm gợi ý ({{ countItem }})</div>
              <div v-if="listSearch.length > 0">
                <div v-for="i in listSearch" :key="i.productId" class="p-2 flex border-b">
                  <div><img :src="i.image" alt="" class="w-14 h-10" /></div>
                  <div class="flex flex-col mx-3">
                    <router-link
                      class="text-sm"
                      :to="{ name: 'productdetail', params: { id: i.productId } }"
                    >
                      {{ i.name }}
                    </router-link>
                    <p>
                      {{ i.discountedPrice ?? i.price | numeral }}₫
                      <s v-if="i.discountedPrice">{{ i.discountedPrice | numeral }}₫</s>
                    </p>
                  </div>
                </div>
              </div>
              <p v-else class="p-2 text-center dark:bg-black dark:text-white text-red-500">
                Không tìm thấy sản phẩm
              </p>
            </div>
          </div>
        </div>

        <!-- Đăng nhập và Giỏ hàng -->
        <div class="flex items-center gap-5 dark:bg-black dark:text-white">
          <!-- Đăng nhập/Đăng ký -->
          <div ref="loginContainer" class="flex justify-center items-center gap-2 relative">
            <a @click="openLogin">
              <VueIcon
                type="mdi"
                :path="mdiAccountOutline"
                class="self-center cursor-pointer"
                size="30"
              />
            </a>
            <div @click="openLogin" class="hidden lg:flex flex-col cursor-pointer">
              <div v-if="!$store.state.user">
                <p class="text-sm text-gray-500 dark:text-white">
                  {{ $t('login.buttons.submit') }} / {{ $t('register.buttons.submit') }}
                </p>
                <p class="flex text-sm font-medium">
                  {{ $t('acount.header') }} <VueIcon type="mdi" :path="mdiChevronDown" size="20" />
                </p>
              </div>
              <div v-else>
                <p class="text-sm text-gray-500 dark:text-white">{{ $t('acount.header') }}</p>
                <p class="flex text-sm font-medium">
                  {{ $store.state.user.email }} <VueIcon type="mdi" :path="mdiChevronDown" size="20" />
                </p>
              </div>
            </div>
            <div
              v-if="$store.state.user"
              :class="login ? 'flex' : 'hidden'"
              class="absolute top-14 text-sm z-20 w-24 border bg-white dark:bg-black dark:text-white text-center shadow-[0px_0px_2px_2px_rgba(0,0,0,0.3)]"
            >
              <ul class="w-full">
                <li v-if="$store.state.user.role === 'Admin'">
                  <router-link
                    :to="{ name: 'adminHome' }"
                    class="block border-b p-2 hover:bg-gray-500 hover:text-white"
                  >
                    Admin
                  </router-link>
                </li>
                <li>
                  <router-link
                    :to="{ name: 'account' }"
                    class="block border-b p-2 hover:bg-gray-500 hover:text-white"
                  >
                    Profile
                  </router-link>
                </li>
                <li
                  @click="logout"
                  class="block border-b p-2 cursor-pointer hover:bg-gray-500 hover:text-white"
                >
                  Logout
                </li>
              </ul>
            </div>
            <div
              v-else
              id="login"
              :class="login ? 'flex' : 'hidden'"
              class="absolute top-14 text-sm w-screen -right-[160px] md:w-[23rem] md:-right-20 z-20 bg-white text-center shadow-[0px_0px_2px_2px_rgba(0,0,0,0.3)]"
            >
              <div class="overflow-hidden w-full">
                <div
                  class="flex w-[300%] transform transition-transform duration-500 ease-out"
                  :style="{
                    transform:
                      recoverPanel === 'login'
                        ? 'translateX(-33.33%)'
                        : recoverPanel === 'register'
                        ? 'translateX(0%)'
                        : 'translateX(-66.66%)',
                  }"
                >
                  <div class="w-[100vw]">
                    <RegisterPanel @openLoginPanel="openLoginPanel" />
                  </div>
                  <div class="w-[100vw]">
                    <LoginPanel
                      @openRecoverPanel="openRecoverPanel"
                      @openRegisterPanel="openRegisterPanel"
                      :getCart="getCart"
                    />
                  </div>
                  <div class="w-[100vw]">
                    <RecoverPanel @openLoginPanel="openLoginPanel" />
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Giỏ hàng -->
          <div ref="cartContainer" class="flex items-center justify-center gap-1 relative">
            <div @click="openCart" class="relative cursor-pointer">
              <a href="javascript:void(0)">
                <VueIcon type="mdi" :path="mdiShoppingOutline" size="30" />
              </a>
              <span
                class="absolute top-0 right-0 flex justify-center items-center w-4 h-4 rounded-full bg-orange-600 text-white"
              >
                {{ countCart }}
              </span>
            </div>
            <p @click="openCart" class="hidden lg:flex cursor-pointer">{{ $t('cart.header') }}</p>
            <div
              id="cart"
              :class="cart ? 'flex' : 'hidden'"
              class="absolute flex-col dark:bg-black dark:text-white border top-14 p-2 text-sm w-screen -right-[100px] md:w-[30rem] md:-right-16 z-20 bg-white text-center shadow-[0px_0px_2px_2px_rgba(0,0,0,0.3)]"
            >
              <p class="uppercase font-bold text-gray-500 dark:text-white py-2 border-b w-full">
                {{ $t('cart.header') }}
              </p>
              <div v-if="$store.state.user">
                <div v-if="carts" class="flex flex-col gap-2 max-h-60 overflow-y-auto">
                  <div v-for="item in carts" :key="item.cartId" class="flex w-full gap-2 p-4 border-b">
                    <img :src="item.product.image" alt="" class="w-16 h-16 object-cover" />
                    <div class="flex w-full flex-col justify-between">
                      <p class="text-sm">{{ item.product.name }}</p>
                      <div class="flex justify-center gap-5 text-sm">
                        <p>{{ item.color }}</p>
                        <p>{{ item.size }}</p>
                      </div>
                      <div class="flex items-center justify-between">
                        <p class="h-5 w-5 bg-gray-300 dark:text-black">{{ item.quantity }}</p>
                        <p class="font-bold">{{ item.totalAmount | numeral }} đ</p>
                        <button @click="deleteCart(item.cartId)">
                          <VueIcon type="mdi" :path="mdiTrashCanOutline" class="text-red-500" />
                        </button>
                      </div>
                    </div>
                  </div>
                </div>
                <div v-else class="flex flex-col items-center py-2 border-b">
                  <VueIcon type="mdi" :path="mdiShoppingOutline" size="50" class="self-center" />
                  <p>{{ $t('cart.description') }}</p>
                </div>
                <div class="p-4 flex flex-col gap-2">
                  <div class="flex justify-between">
                    <p class="uppercase">{{ $t('cart.total_Amount') }}:</p>
                    <p class="text-red-500 font-bold">{{ totalAmount | numeral }} <u>đ</u></p>
                  </div>
                  <div class="flex gap-2 justify-between text-white text-sm uppercase">
                    <router-link :to="{ name: 'cart' }" class="w-1/2 bg-gray-600 py-2">
                      {{ $t('cart.buttons.show_cart') }}
                    </router-link>
                    <router-link :to="{ name: 'checkout' }" class="w-1/2 bg-blue-800 py-2">
                      {{ $t('cart.buttons.payment') }}
                    </router-link>
                  </div>
                </div>
              </div>
              <div v-else class="p-2">Bạn chưa đăng nhập</div>
            </div>
          </div>

          <!-- Dark mode toggle -->
          <div class="flex">
            <toggle-button
              v-model="isDarkMode"
              class="border rounded-xl"
              color="#000000"
              :labels="{ checked: 'Dark', unchecked: 'Night' }"
              width="60"
              @change="toggleDarkMode"
            />
          </div>
        </div>
      </div>

      <!-- Ô tìm kiếm (Mobile) -->
      <div ref="searchMobileContainer" class="w-full py-3 dark:bg-black dark:text-white">
        <div class="flex md:hidden items-center relative border-2 border-grayMang-100 dark:bg-black dark:text-white w-full h-10">
          <input
            @focus="showSuggestions = true"
            type="text"
            v-model="searchValue"
            placeholder="Tìm kiếm sản phẩm..."
            class="outline-none dark:bg-black dark:text-white w-2/3"
          />
          <router-link
            :to="{ name: 'search', query: { q: searchValue } }"
            class="absolute right-0 bg-gray-700 dark:bg-white h-10 w-10 flex justify-center items-center"
          >
            <VueIcon type="mdi" :path="mdiMagnify" class="text-white dark:text-black" />
          </router-link>
          <div
            v-if="showSuggestions"
            class="absolute top-10 bg-white dark:bg-black dark:text-white max-h-80 overflow-x-auto w-full z-10 shadow-md rounded-sm"
          >
            <div class="p-1 bg-gray-200 dark:text-black">Sản phẩm gợi ý ({{ countItem }})</div>
            <div v-if="listSearch.length > 0">
              <div v-for="i in listSearch" :key="i.productId" class="p-2 flex border-b">
                <div><img :src="i.image" alt="" class="w-14 h-10" /></div>
                <div class="flex flex-col mx-3">
                  <router-link
                    class="text-sm"
                    :to="{ name: 'productdetail', params: { id: i.productId } }"
                  >
                    {{ i.name }}
                  </router-link>
                  <p>
                    {{ i.discountedPrice ?? i.price | numeral }}₫
                    <s v-if="i.discountedPrice">{{ i.discountedPrice | numeral }}₫</s>
                  </p>
                </div>
              </div>
            </div>
            <p v-else class="p-2 text-center text-red-500">Không tìm thấy sản phẩm</p>
          </div>
        </div>
        <ul class="hidden lg:flex gap-5 text-sm">
          <li>
            <CategoryMenu
              v-if="category && category.length"
              :category="category"
              ulClass="hidden lg:flex gap-5 px-0 text-sm"
              liClass="flex items-center py-2 relative group"
              subUlClass="invisible opacity-0 group-hover:visible group-hover:opacity-100 absolute w-32 flex-col z-50 top-full left-0 bg-white dark:bg-black dark:text-white border shadow-[0px_0px_2px_2px_rgba(0,0,0,0.3)] transition-all duration-200"
            />
          </li>
          <li class="flex items-center"><router-link :to="{ name: 'about' }">Dịch Vụ</router-link></li>
          <li class="flex items-center"><router-link :to="{ name: 'about' }">Về chúng tôi</router-link></li>
          <li class="flex items-center"><router-link :to="{ name: 'contact' }">Liên hệ</router-link></li>
        </ul>
      </div>
    </nav>

    <!-- Navbar Sticky -->
    <nav
      v-if="isSticky"
      class="navbar-sticky fixed w-full z-50 bg-white dark:bg-black shadow transition-all duration-300 opacity-100 translate-y-0"
    >
      <div class="container mx-auto md:px-40">
        <div class="flex items-center justify-between py-2">
          <!-- Logo -->
          <router-link :to="{ name: 'home' }">
            <img src="/assets/image/logo.png" alt="Logo" class="h-10" />
          </router-link>
          <!-- Danh mục -->
          <div ref="searchContainerSticky" class="w-80 hidden md:flex">
            <div
              class="flex items-center relative dark:bg-black dark:text-white border-2 p-1 border-gray-100 w-80 h-10 rounded focus-within:border-gray-400"
            >
              <input
                type="text"
                v-model="searchValue"
                placeholder="Tìm kiếm sản phẩm..."
                class="outline-none w-2/3 dark:bg-black dark:text-white rounded"
                @focus="showSuggestions = true"
              />
              <router-link
                :to="{ name: 'search', query: { q: searchValue } }"
                class="absolute right-0 bg-gray-700 dark:bg-white h-10 w-10 flex justify-center items-center rounded border-none"
              >
                <VueIcon type="mdi" :path="mdiMagnify" class="dark:text-black text-white" />
              </router-link>
              <div
                v-if="showSuggestions"
                class="absolute top-10 bg-white dark:bg-black dark:text-white max-h-80 overflow-x-auto w-full z-10 shadow-md rounded-sm"
              >
                <div class="p-1 bg-gray-200 dark:text-black">Sản phẩm gợi ý ({{ countItem }})</div>
                <div v-if="listSearch.length > 0">
                  <div v-for="i in listSearch" :key="i.productId" class="p-2 flex border-b">
                    <div><img :src="i.image" alt="" class="w-14 h-10" /></div>
                    <div class="flex flex-col mx-3">
                      <router-link
                        class="text-sm"
                        :to="{ name: 'productdetail', params: { id: i.productId } }"
                      >
                        {{ i.name }}
                      </router-link>
                      <p>
                        {{ i.discountedPrice ?? i.price | numeral }}₫
                        <s v-if="i.discountedPrice">{{ i.discountedPrice | numeral }}₫</s>
                      </p>
                    </div>
                  </div>
                </div>
                <p v-else class="p-2 text-center dark:bg-black dark:text-white text-red-500">
                  Không tìm thấy sản phẩm
                </p>
              </div>
            </div>
          </div>
          <ul class="hidden lg:flex gap-5 text-sm">
            <li>
              <CategoryMenu
                v-if="category && category.length"
                :category="category"
                ulClass="hidden lg:flex gap-5 px-0 text-sm"
                liClass="flex items-center py-2 relative group"
                subUlClass="invisible opacity-0 group-hover:visible group-hover:opacity-100 absolute w-32 flex-col z-50 top-full left-0 bg-white dark:bg-black dark:text-white border shadow-[0px_0px_2px_2px_rgba(0,0,0,0.3)] transition-all duration-200"
              />
            </li>
            <li class="flex items-center"><router-link :to="{ name: 'about' }">Dịch Vụ</router-link></li>
            <li class="flex items-center"><router-link :to="{ name: 'about' }">Về chúng tôi</router-link></li>
            <li class="flex items-center"><router-link :to="{ name: 'contact' }">Liên hệ</router-link></li>
          </ul>
          <!-- Đăng nhập/Đăng ký và Giỏ hàng -->
          <div class="flex items-center gap-2">
            <div ref="loginContainerSticky" @click="openLogin" class="relative cursor-pointer">
              <VueIcon type="mdi" :path="mdiAccountOutline" size="28" />
              <div
                v-if="$store.state.user"
                :class="login ? 'flex' : 'hidden'"
                class="absolute top-14 text-sm z-20 w-24 border bg-white dark:bg-black dark:text-white text-center shadow-[0px_0px_2px_2px_rgba(0,0,0,0.3)]"
              >
                <ul class="w-full">
                  <li v-if="$store.state.user.role === 'Admin'">
                    <router-link
                      :to="{ name: 'adminHome' }"
                      class="block border-b p-2 hover:bg-gray-500 hover:text-white"
                    >
                      Admin
                    rubbing
                    </router-link>
                  </li>
                  <li>
                    <router-link
                      :to="{ name: 'account' }"
                      class="block border-b p-2 hover:bg-gray-500 hover:text-white"
                    >
                      Profile
                    </router-link>
                  </li>
                  <li
                    @click="logout"
                    class="block border-b p-2 cursor-pointer hover:bg-gray-500 hover:text-white"
                  >
                    Logout
                  </li>
                </ul>
              </div>
              <div
                v-else
                id="login"
                :class="login ? 'flex' : 'hidden'"
                class="absolute top-14 text-sm w-screen -right-[160px] md:w-[23rem] md:-right-20 z-20 bg-white text-center shadow-[0px_0px_2px_2px_rgba(0,0,0,0.3)]"
              >
                <div class="overflow-hidden w-full">
                  <div
                    class="flex w-[300%] transform transition-transform duration-500 ease-out"
                    :style="{
                      transform:
                        recoverPanel === 'login'
                          ? 'translateX(-33.33%)'
                          : recoverPanel === 'register'
                          ? 'translateX(0%)'
                          : 'translateX(-66.66%)',
                    }"
                  >
                    <div class="w-[100vw]">
                      <RegisterPanel @openLoginPanel="openLoginPanel" />
                    </div>
                    <div class="w-[100vw]">
                      <LoginPanel
                        @openRecoverPanel="openRecoverPanel"
                        @openRegisterPanel="openRegisterPanel"
                        :getCart="getCart"
                      />
                    </div>
                    <div class="w-[100vw]">
                      <RecoverPanel @openLoginPanel="openLoginPanel" />
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div ref="cartContainerSticky" @click="openCart" class="relative cursor-pointer">
              <VueIcon type="mdi" :path="mdiShoppingOutline" size="28" />
              <span
                class="absolute -top-2 -right-2 w-4 h-4 rounded-full bg-orange-600 text-white text-xs flex items-center justify-center"
              >
                {{ countCart }}
              </span>
              <div
                id="cart"
                :class="cart ? 'flex' : 'hidden'"
                class="absolute flex-col dark:bg-black dark:text-white border top-14 p-2 text-sm w-screen -right-[100px] md:w-[30rem] md:-right-16 z-20 bg-white text-center shadow-[0px_0px_2px_2px_rgba(0,0,0,0.3)]"
              >
                <p class="uppercase font-bold text-gray-500 dark:text-white py-2 border-b w-full">
                  {{ $t('cart.header') }}
                </p>
                <div v-if="$store.state.user">
                  <div v-if="carts" class="flex flex-col gap-2 max-h-60 overflow-y-auto">
                    <div v-for="item in carts" :key="item.cartId" class="flex w-full gap-2 p-4 border-b">
                      <img :src="item.product.image" alt="" class="w-16 h-16 object-cover" />
                      <div class="flex w-full flex-col justify-between">
                        <p class="text-sm">{{ item.product.name }}</p>
                        <div class="flex justify-center gap-5 text-sm">
                          <p>{{ item.color }}</p>
                          <p>{{ item.size }}</p>
                        </div>
                        <div class="flex items-center justify-between">
                          <p class="h-5 w-5 bg-gray-300 dark:text-black">{{ item.quantity }}</p>
                          <p class="font-bold">{{ item.totalAmount | numeral }} đ</p>
                          <button @click="deleteCart(item.cartId)">
                            <VueIcon type="mdi" :path="mdiTrashCanOutline" class="text-red-500" />
                          </button>
                        </div>
                      </div>
                    </div>
                  </div>
                  <div v-else class="flex flex-col items-center py-2 border-b">
                    <VueIcon type="mdi" :path="mdiShoppingOutline" size="50" class="self-center" />
                    <p>{{ $t('cart.description') }}</p>
                  </div>
                  <div class="p-4 flex flex-col gap-2">
                    <div class="flex justify-between">
                      <p class="uppercase">{{ $t('cart.total_Amount') }}:</p>
                      <p class="text-red-500 font-bold">{{ totalAmount | numeral }} <u>đ</u></p>
                    </div>
                    <div class="flex gap-2 justify-between text-white text-sm uppercase">
                      <router-link :to="{ name: 'cart' }" class="w-1/2 bg-gray-600 py-2">
                        {{ $t('cart.buttons.show_cart') }}
                      </router-link>
                      <router-link :to="{ name: 'checkout' }" class="w-1/2 bg-blue-800 py-2">
                        {{ $t('cart.buttons.payment') }}
                      </router-link>
                    </div>
                  </div>
                </div>
                <div v-else class="p-2">Bạn chưa đăng nhập</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </nav>
  </div>
</template>

<script>
import {
  mdiMagnify,
  mdiAccountOutline,
  mdiChevronDown,
  mdiTrashCanOutline,
  mdiShoppingOutline,
  mdiChevronRight,
  mdiHeart,
  mdiViewHeadline,
  mdiChevronUp,
  mdiClose,
  mdiWeatherNight,
} from '@mdi/js';
import RecoverPanel from '../recover/RecoverPanel.vue';
import LoginPanel from '../login/LoginPanel.vue';
import RegisterPanel from '../register/RegisterPanel.vue';
import CategoryMenu from './CategoryMenu.vue';
import axios from 'axios';

export default {
  name: 'NavBar',
  props: ['countCart', 'getCart', 'carts', 'totalAmount'],
  components: { RecoverPanel, LoginPanel, RegisterPanel, CategoryMenu },
  data() {
    return {
      isSticky: false,
      isDarkMode: localStorage.getItem('darkMode') === 'true' || false,
      menu: false,
      openMenuIndexes: [],
      login: false,
      cart: false,
      recoverPanel: 'login',
      category: '',
      searchValue: '',
      listSearch: '',
      countItem: 0,
      showSuggestions: false,
      mdiMagnify,
      mdiAccountOutline,
      mdiChevronDown,
      mdiShoppingOutline,
      mdiHeart,
      mdiViewHeadline,
      mdiChevronUp,
      mdiClose,
      mdiWeatherNight,
      mdiChevronRight,
      mdiTrashCanOutline,
      lastScrollY: 0,
      scrollTimeout: null,
    };
  },
  mounted() {
    this.getCategory();
    window.addEventListener('scroll', this.handleSmartScroll);
    document.addEventListener('click', this.handleClickOutside);
    document.addEventListener('keydown', this.handleKeyDown);
  },
  beforeDestroy() {
    window.removeEventListener('scroll', this.handleSmartScroll);
    document.removeEventListener('click', this.handleClickOutside);
    document.removeEventListener('keydown', this.handleKeyDown);
  },
  watch: {
    searchValue(newValue) {
      this.showSuggestions = newValue !== '';
      this.getproduct();
    },
  },
  methods: {
    handleClickOutside(event) {
      // Kiểm tra click ngoài các khu vực: tìm kiếm (mobile + desktop), đăng nhập, giỏ hàng
      const isOutsideSearch =
        (!this.$refs.searchContainer || !this.$refs.searchContainer.contains(event.target)) &&
        (!this.$refs.searchMobileContainer || !this.$refs.searchMobileContainer.contains(event.target)) &&
        (!this.$refs.searchContainerSticky || !this.$refs.searchContainerSticky.contains(event.target));
      const isOutsideLogin =
        (!this.$refs.loginContainer || !this.$refs.loginContainer.contains(event.target)) &&
        (!this.$refs.loginContainerSticky || !this.$refs.loginContainerSticky.contains(event.target));
      const isOutsideCart =
        (!this.$refs.cartContainer || !this.$refs.cartContainer.contains(event.target)) &&
        (!this.$refs.cartContainerSticky || !this.$refs.cartContainerSticky.contains(event.target));
      const isOutsideMenu = !this.$refs.menuContainer || !this.$refs.menuContainer.contains(event.target);

      // Ẩn các thành phần nếu click ra ngoài
      if (isOutsideSearch) {
        this.showSuggestions = false;
      }
      if (isOutsideLogin) {
        this.login = false;
      }
      if (isOutsideCart) {
        this.cart = false;
      }
      if (isOutsideMenu) {
        this.menu = false;
      }
    },
    handleKeyDown(event) {
      // Ẩn tất cả khi nhấn phím Escape
      if (event.key === 'Escape') {
        this.showSuggestions = false;
        this.login = false;
        this.cart = false;
        this.menu = false;
      }
    },
    toggleDarkMode() {
      this.$emit('toggle-dark-mode');
    },
    handleSmartScroll() {
      const currentY = window.scrollY;
      if (currentY > this.lastScrollY) {
        if (!this.scrollTimeout) {
          this.scrollTimeout = setTimeout(() => {
            this.isSticky = currentY > 40;
            this.scrollTimeout = null;
            this.lastScrollY = currentY;
          }, 300);
        }
      } else {
        if (this.scrollTimeout) {
          clearTimeout(this.scrollTimeout);
          this.scrollTimeout = null;
        }
        this.isSticky = currentY > 40;
        this.lastScrollY = currentY;
      }
    },
    async getproduct() {
      try {
        const res = await axios.get(`/Product/search?q=${this.searchValue}`);
        this.listSearch = res.data.products;
        this.countItem = res.data.total;
      } catch (err) {
        console.log(err);
      }
    },
    openMenu() {
      this.menu = !this.menu;
      this.login = false;
      this.cart = false;
      this.showSuggestions = false;
    },
    toggleMenu(index) {
      const menuIndex = this.openMenuIndexes.indexOf(index);
      if (menuIndex > -1) {
        this.openMenuIndexes.splice(menuIndex, 1);
      } else {
        this.openMenuIndexes.push(index);
      }
    },
    isMenuOpen(index) {
      return this.openMenuIndexes.includes(index);
    },
    openLogin() {
      this.login = !this.login;
      this.cart = false;
      this.menu = false;
      this.showSuggestions = false;
    },
    openLoginPanel(data) {
      this.recoverPanel = data;
    },
    openRegisterPanel(data) {
      this.recoverPanel = data;
    },
    openRecoverPanel(data) {
      this.recoverPanel = data;
    },
    openCart() {
      this.cart = !this.cart;
      this.login = false;
      this.menu = false;
      this.showSuggestions = false;
    },
    logout() {
      this.$store.commit('LOGOUT');
      this.getCart();
    },
    async getCategory() {
      try {
        const res = await axios.get('/CategoryType/getAll');
        this.category = res.data;
      } catch (err) {
        console.log(err);
      }
    },
    async deleteCart(id) {
      try {
        await axios.delete(`/Cart/delete/${id}`);
        this.$toast.success(`Xóa thành công`, {
          position: 'top-right',
          timeout: 5000,
        });
        this.getCart();
      } catch (err) {
        console.log(err);
      }
    },
  },
};
</script>

<style lang="css">
#menu::before {
  content: '';
  width: 0;
  height: 0;
  position: absolute;
  top: -20px;
  left: 30px;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  border-bottom: 20px solid white;
  display: inline-block;
  filter: drop-shadow(0px -2px rgba(0, 0, 0, 0.3));
}
#login::before,
#cart::before {
  content: '';
  width: 0;
  height: 0;
  position: absolute;
  top: -14px;
  right: 100px;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  border-bottom: 15px solid white;
  display: inline-block;
  filter: drop-shadow(0px -2px rgba(0, 0, 0, 0.3));
}
.navbar-normal {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 0.3s, transform 0.3s;
}
.navbar-sticky {
  opacity: 1;
  transform: translateY(0);
  transition: opacity 0.3s, transform 0.3s;
}
.navbar-normal-leave-active,
.navbar-sticky-leave-active {
  transition: opacity 0.3s, transform 0.3s;
}
.navbar-normal-leave-to,
.navbar-sticky-leave-to {
  opacity: 0;
  transform: translateY(-20px);
}
.navbar-normal-enter-active,
.navbar-sticky-enter-active {
  transition: opacity 0.3s, transform 0.3s;
}
.navbar-normal-enter,
.navbar-sticky-enter {
  opacity: 0;
  transform: translateY(-20px);
}
</style>