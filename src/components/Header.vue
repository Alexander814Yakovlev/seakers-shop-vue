<script setup>
import { inject } from 'vue';

const { toggleDrawer, cartTotal } = inject('cart')
</script>

<template>
    <header class="flex justify-between border-b border-slate-300 px-10 py-8 flex-wrap gap-5">
        <RouterLink to="/" class="flex items-center gap-4">
            <img class="w-10" src="/logo.png" alt="Logo">
            <div>
                <h2 class="text-xl font-bold uppercase">Vue Seneakers</h2>
                <p class="text-slate-400">Магазин лучших кроссовок</p>
            </div>
        </RouterLink>
        <ul class="flex flex-1 sm:flex-none justify-center  items-center gap-10">
            <li class="flex flex-col sm:flex-row items-center gap-3 cursor-pointer" @click="toggleDrawer">
                <img src="/cart.svg" alt="cart">
                <b class="text-gray-500 hover:text-black">{{ cartTotal }} &#8381;</b>
            </li>
            <li class="flex items-center gap-3 cursor-pointer">
                <RouterLink to="/favorites" class="flex flex-col sm:flex-row items-center gap-3">
                    <img src="/heart.svg" alt="heart">
                    <span class="text-gray-500 hover:text-black">Закладки</span>
                </RouterLink>

            </li>
            <!-- <li class="flex items-center gap-3 cursor-pointer">
                <img src="/profile.svg" alt="profile">
                <span class="text-gray-500 hover:text-black">Профиль</span>
            </li> -->
        </ul>
    </header>
</template>