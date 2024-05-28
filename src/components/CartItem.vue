<script setup>
defineProps({
    title: String,
    price: Number,
    imageUrl: String,
    removeFromCart: Function,
})

</script>

<template>
    <div class="flex border border-slate-200 p-4 rounded-xl gap-4">
        <img class="w-16 h-16" :src="imageUrl" alt="item">
        <div class="flex flex-col">
            <p class="block w-full">{{ title }}</p>
            <div class="flex justify-between mt-2">
                <b>{{ price }} &#8381;</b>
                <img class="opacity-50 hover:opacity-100 cursor-pointer transition" src="/close.svg" alt="X"
                    @click="removeFromCart" />
            </div>
        </div>
    </div>
</template>