<script setup>
import Card from './Card.vue'

defineProps ({
  items: Array,
  isFavorites: Boolean
  
})

const emit = defineEmits(['addToFavorites', 'addToCart'])

</script>


<template>
    <div class="grid grid-cols-4 gap-5" v-auto-animate>
        <Card 
          v-for="item in items"
          :key="item.id"
          :isAdded="item.isAdded" 
          :isFavorite="item.isFavorite" 
          :title="item.title"
          :imageUrl="item.imageUrl" 
          :onClickFavorite="isFavorites ? null : ()=> emit('addToFavorites', item)"
          :onClickAdd="isFavorites ? null : ()=> emit('addToCart', item)"
          :price="item.price"
        />
        
      </div>
</template>