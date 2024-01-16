<script setup>
    defineProps({
        title: String,
        imageUrl: String,
        price: Number,
        isFavorite: Boolean,
        isAdded: Boolean
    })
</script>

<template>
    <div class="relative border bg-white border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl" >
        <img 
            :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'" 
            alt="like1" 
            class="absolute top-8 left-8"
        >
        <img :src="imageUrl" alt="sneaker">

        <p class="mt-2">{{ title }}</p>

        <div class="flex justify-between mt-5">
            <div class="flex flex-col">
                <span class="text-s;ate-400">Price</span>
                <b>{{ price }}</b>
            </div>

            <img :src="!isAdded ? '/plus.svg' : '/checked.svg'" alt="plus">
        </div>
    </div>
</template>