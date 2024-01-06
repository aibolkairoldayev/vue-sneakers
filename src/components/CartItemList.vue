<script setup>
import CartItem from './CartItem.vue'

</script>


<template>
    <div class="flex flex-col gap-5">
        <CartItem isAdded="true" isFavorite="true" title="Nike Blazer Mid Suede for men" price="12500 tg"/>
      
        <CartItem/>
      </div>
</template>