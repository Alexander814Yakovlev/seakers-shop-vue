<script setup>
import { inject } from 'vue';
import CartItem from './CartItem.vue';
const { cart, addToCart } = inject('cart')
</script>

<template>
    <div v-auto-animate class="flex flex-col gap-4">
        <CartItem v-for="item of cart" :key="item.id" :title="item.title" :price="item.price" :imageUrl="item.imageUrl"
            :removeFromCart="() => addToCart(item)" />
    </div>

</template>