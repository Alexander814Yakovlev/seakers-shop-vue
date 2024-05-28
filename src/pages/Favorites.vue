<script setup>
import axios from 'axios';
import { ref, inject, onMounted } from 'vue';
import CardList from "../components/CardList.vue"
const { addToCart } = inject('cart')
const favorites = ref([])

const fetchFavoritesList = async () => {
    try {
        const { data } = await axios.get('https://f0140d44a488f752.mokky.dev/favorites')
        favorites.value = data
    } catch (err) {
        console.log(err);
    }
}

const removeFromFavorites = async (item) => {
    await axios.delete(`https://f0140d44a488f752.mokky.dev/favorites/${item.id}`)
    fetchFavoritesList()
}

onMounted(fetchFavoritesList)
</script>

<template>
    <div class="flex gap-5 items-center mb-8">
        <RouterLink to="/"><img src="/chevron-left.svg" alt="back"></RouterLink>
        <h2 class="text-3xl font-bold">Мои закладки</h2>
    </div>
    <CardList :items="favorites" @addFavorite="removeFromFavorites" @addToCart="addToCart" />
</template>