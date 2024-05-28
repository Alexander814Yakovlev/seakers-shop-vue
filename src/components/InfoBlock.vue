<script setup>
defineProps({
    title: String,
    description: String,
    imageUrl: String,
})

const emit = defineEmits('onClickBack')
</script>

<template>
    <div class="flex flex-col gap-8 items-center text-center w-72 mx-auto">
        <img width="100" height="100" :src="imageUrl" alt="Info image">
        <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
        <p class="text-gray-400 mt-2">{{ description }}</p>
        <button @click="emit('onClickBack')"
            class="bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 transition active:bg-lime-700 cursor-pointer my-7">
            Вернуться назад
        </button>
    </div>

</template>