<script setup>
import { inject } from 'vue';
import Card from './Card.vue';
import Preloader from './Preloader.vue';

defineProps({
    items: Array,
})

const { addToCart } = inject('cart')
const { addFavorite } = inject('items')
</script>

<template>
    <div v-auto-animate v-if="items.length" class="grid grid-responsive gap-5 mt-10">
        <Card v-for="item of items" :key="item.id" :title="item.title" :imageUrl="item.imageUrl" :price="item.price"
            :isFavorite="item.isFavorite" :addFavorite="() => addFavorite(item)" :isAdded="item.isAdded"
            :addToCart="() => addToCart(item)" />
    </div>
    <Preloader v-else />
</template>

<style scoped>
.grid-responsive {
    grid-template-columns: repeat(auto-fill, minmax(18rem, 1fr));
}
</style>