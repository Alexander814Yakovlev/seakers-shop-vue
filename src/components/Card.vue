<script setup>
defineProps({
    imageUrl: String,
    title: String,
    price: Number,
    isFavorite: Boolean,
    isAdded: Boolean,
    onFavorite: Function,
    addFavorite: Function,
    addToCart: Function,
})

</script>

<template>
    <div>
        <div
            class="flex flex-col relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl trnsition">
            <img class="absolute top-8 left-8" :src="isFavorite ? './like-2.svg' : './like-1.svg'" alt="like"
                @click="addFavorite">
            <img :src="imageUrl" alt="sneakers-1">
            <p class="block mt-2 w-full ">{{ title }}</p>
            <div class="flex justify-between w-full mt-5 ">
                <div class="flex flex-col">
                    <span class="text-slate-400">Цена</span>
                    <span class="font-bold">{{ price }} &#8381;</span>
                </div>
                <img class="self-end" :src="!isAdded ? './plus.svg' : './checked.svg'" alt="plus" @click="addToCart">
            </div>
        </div>
    </div>
</template>
