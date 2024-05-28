<script setup>
import { ref, reactive, watch, onMounted, inject } from 'vue';
import axios from 'axios';
import debounce from 'lodash.debounce';
import CardList from '@/components/CardList.vue';

const { cart, addToCart } = inject('cart')
const { addFavorite } = inject('items')

let items = ref([])
let filters = reactive({
    searchQuery: '',
    sortBy: ''
})


const getItems = () => {
    let params = {
        sortBy: filters.sortBy,
    }
    if (filters.searchQuery) {
        params.title = `*${filters.searchQuery}*`
    }
    axios.get('https://f0140d44a488f752.mokky.dev/items', {
        params: { ...params }
    })
        .then((response) => items.value = response.data.map(obj => ({
            ...obj,
            favoriteId: null,
            isFavorite: false,
            isAdded: cart.value.map(item => item.id).includes(obj.id),
        })))
}

const getFavorites = () => {
    axios.get('https://f0140d44a488f752.mokky.dev/favorites')
        .then(resp => items.value = items.value.map(obj => {
            const favorite = resp.data.find(fav => fav.parentId === obj.id);
            if (!favorite) {
                return obj;
            } else {
                return {
                    ...obj,
                    isFavorite: true,
                    favoriteId: favorite.id,
                }
            }
        }))
}


const onSelectChange = (e) => {
    filters.sortBy = e.target.value
}
const onInputChange = debounce((e) => {
    filters.searchQuery = e.target.value
}, 1000)

watch(filters, () => {
    getItems();
    getFavorites();
})
watch(cart, () => {
    localStorage.sneakersOrder = JSON.stringify(cart.value)
    items.value = items.value.map(obj => ({
        ...obj,
        isAdded: cart.value.map(item => item.id).includes(obj.id),
    }))
},
    { deep: true })

onMounted(() => {
    getItems();
    getFavorites();
})

</script>


<template>
    <div class="flex justify-between flex-wrap gap-5 items-center mb-8">
        <h2 class="text-3xl font-bold">Все кроссовки</h2>
        <div class="flex gap-3 flex-wrap">
            <select class="bg-white py-2 px-3 border rounded-md outline-none w-full md:w-40" @change="onSelectChange">
                <option value="" disabled selected>Сортировка</option>
                <option value="title">По названию</option>
                <option value="price">Сначала дешёвые</option>
                <option value="-price">Сначала дорогие</option>
            </select>
            <div class="relative w-full md:w-56">
                <img class="absolute left-4 top-3.5" src="/search.svg" alt="search">
                <input class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400 w-full" type="text"
                    placeholder="Поиск..." @input="onInputChange">
            </div>
        </div>
    </div>
    <CardList :items="items" @addFavorite="addFavorite" @addToCart="addToCart" />
</template>