<script setup>
import Card from './Card.vue'

defineProps ({
  items: {
    type:Array,
  }
})

const emit = defineEmits(['addToFavorites', 'addToCart'])

</script>


<template>
    <div class="grid grid-cols-4 gap-5">
        <Card 
          v-for="item in items"
          :key="item.id"
          :isAdded="item.isAdded" 
          :isFavorite="item.isFavorite" 
          :title="item.title"
          :imageUrl="item.imageUrl" 
          :onClickFavorite="()=>{emit('addToFavorites', item)}"
          :onClickAdd="()=>{emit('addToCart', item)}"
          :price="item.price"
        />
        
      </div>
</template>