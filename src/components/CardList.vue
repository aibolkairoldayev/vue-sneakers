<script setup>
import Card from './Card.vue'

defineProps ({
  items:[]
})

const emit = defineEmits(['addToFavorites'])

</script>


<template>
    <div class="grid grid-cols-4 gap-5">
        <Card 
          v-for="item in items"
          :key="item.id"
          isAdded="true" 
          :isFavorite="item.isFavorite" 
          :title="item.title"
          :imageUrl="item.imageUrl" 
          :onClickFavorite="()=>{emit('addToFavorites', item)}"
          :price="item.price"
        />
        
      </div>
</template>