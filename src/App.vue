<script setup>
  import Header from './components/Header.vue'
  import CardList from './components/CardList.vue'
  import Drawer from './components/Drawer.vue'
</script>

<template>
  <Drawer/>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 ">
    <Header/>
    <div class="p-10">
      <div class="flex justify-between">
        <h2 class="text-3xl font-bold mb-8">All sneakers</h2>

        <div class="flex gap-4">
          <select class="py-2 px-3 border rounded-md outline-none" name="" id="">
          <option value="">По названию</option>
          <option value="">Сначала дешевые</option>
          <option value="">Сначала дорогие</option>
        </select>

        <div class="relative">
          <img class="absolute left-4 top-3" src="/search.svg" alt="search">
          <input 
          type="text"
          placeholder="Поиск" 
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus: border-gray-400"
        >
        </div>
        </div>
      </div>
      <CardList/>
    </div>
  </div>
</template>

<style scoped>
</style>
