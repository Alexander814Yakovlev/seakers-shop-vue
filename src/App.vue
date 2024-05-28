<script setup>
import { ref, onMounted, provide, computed } from 'vue';
import axios from 'axios';
import Header from './components/Header.vue';
import Drawer from './components/Drawer.vue';


let cart = ref([])
let isDrawer = ref(false)
let cartTotal = computed(() => cart.value.reduce((x, y) => x + y.price, 0))
let tax = computed(() => Math.round(cartTotal.value * 0.05))


const addFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        ...item,
        parentId: item.id,
        isFavorite: true,
      }
      item.isFavorite = true
      const { data } = await axios.post('https://f0140d44a488f752.mokky.dev/favorites', obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://f0140d44a488f752.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  }
  catch (err) {
    console.log(err)
  }
}

const addToCart = (item) => {
  if (!item.isAdded) {
    cart.value.push(item)
    item.isAdded = true
  } else {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
  }
}

const toggleDrawer = () => {
  isDrawer.value = !isDrawer.value
}

onMounted(() => {
  if (localStorage.getItem('sneakersOrder')) {
    cart.value = JSON.parse(localStorage.sneakersOrder)
  }
}
)

provide('items', {
  addFavorite,
})
provide('cart', {
  cart,
  cartTotal,
  tax,
  addToCart,
  toggleDrawer,
})

</script>

<template>
  <Drawer v-if="isDrawer" v-auto-animate />
  <div class="bg-white w-4/5 m-auto h-max rounded-xl shadow-xl my-10 pb-6">
    <Header />
    <div class="p-0.5 pt-10 sm:p-10">
      <RouterView />
    </div>
  </div>
</template>