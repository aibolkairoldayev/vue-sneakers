<script setup>
  import { computed, onMounted, provide, ref, watch } from 'vue';
  import axios from 'axios';

  import Header from './components/Header.vue'
  import Drawer from './components/Drawer.vue'


  const cart = ref([]);
  const isCreatingOrder = ref(false)

  const drawerOpen = ref(false)

  const totalPrice = computed(()=> cart.value.reduce((acc, item )=>acc + item.price, 0))

  const cartIsEmpty = computed(()=> cart.value.length===0)

  const cartButtonDisabled = computed(()=> isCreatingOrder.value || cartIsEmpty.value)

  const openDrawer = () => {
    drawerOpen.value = true
  } 

  const closeDrawer = () => {
    drawerOpen.value = false
  } 





  const removeFromCart = (item) => {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
  }

  const createOrder = async ()=> {
    try{
      isCreatingOrder.value = true

      const {data} = await axios.post('https://7687af431bcf909f.mokky.dev/orders', {
        items: cart.value,
        totalPrice: totalPrice.value
      })

      cart.value=[]

      return data
    } catch(err) {
      console.log(err)
    } finally {
      isCreatingOrder.value = true
    }
  }
  watch(
    cart,
    ()=> {
      localStorage.setItem('cart', JSON.stringify(cart.value))
    },
    {deep:true}
  )

  provide('cart', {
    cart,
    openDrawer,
    closeDrawer,
    removeFromCart
  })

</script>

<template>
  <Drawer v-if="drawerOpen" :totalPrice="totalPrice" @create-order="createOrder" :button-disabled="cartButtonDisabled"/>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 ">
    <Header @open-drawer="openDrawer" :totalPrice="totalPrice"/>
    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped>
</style>
