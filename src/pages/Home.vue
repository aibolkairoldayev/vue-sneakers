<script setup>
    import CardList from '../components/CardList.vue'

    import axios from 'axios';
    import { computed, inject, onMounted, provide, reactive, ref, watch } from 'vue';

    const items = ref([]);

    const filters = reactive({
        sortBy: 'title',
        searchQuery: ''
    })

      onMounted(async ()=> {
        const localCart = localStorage.getItem('cart')
        cart.value = localCart ? JSON.parse(localCart) : []

        await fetchItems()
        await  fetchFavorites()

        items.value = items.value.map((item)=> ({
        ...item,
        isAdded: cart.value.some((cartItem)=>cartItem.id === item.id)
        }))
    })

    const fetchItems = async ()=> {
    try{
      const params = {
        sortBy: filters.sortBy
      }

      if(filters.searchQuery) {
        params.title = `*${filters.searchQuery}*`
      }

      const {data} = await axios.get('https://7687af431bcf909f.mokky.dev/items', {
        params
      })


      items.value = data.map((obj) => ({
        ...obj,
        isFavorite: false,
        favoriteId: null,
        isAdded: false
      }))


    } catch (err) {
      console.log(err)
    }
  }

    const onClickAddPlus = (item) => {
    if(!item.isAdded) {
      addToCart(item)
    } else {
     removeFromCart(item)
    }
  }

    const fetchFavorites = async ()=> {
    try {
      const {data: favorites} = await axios.get('https://7687af431bcf909f.mokky.dev/favorites')

      items.value = items.value.map((item) => {
        const favorite = favorites.find((favorite)=>favorite.parentId === item.id)

        if(!favorite) {
          return item
        }

        return {
          ...item,
          isFavorite: true,
          favoriteId: favorite.id
        }
      })
    } catch(err) {
      console.log(err)
    }
  }

    const addToFavorites = async (item) => {
    try{
      if(!item.isFavorite) {
        const obj = {
          parentId: item.id
        }

        item.isFavorite = true

        const {data} = await axios.post('https://7687af431bcf909f.mokky.dev/favorites', obj)

        item.favoriteId = data.id
      } else {
        item.isFavorite = false
        await axios.delete(`https://7687af431bcf909f.mokky.dev/favorites/${item.favoriteId}`)
        item.favoriteId = null
      }
    } catch(err) {
      console.log(err)
    }
  }

    const addToCart = (item) => {
    cart.value.push(item)
      item.isAdded = true
  }

  const onChangeSelect = (event) => {
    filters.sortBy = event.target.value
  }

  const onChangeSearchInput = (event) => {
    filters.searchQuery = event.target.value
  }

    const {cart, removeFromCart} = inject('cart')

      watch(cart, ()=>{
    items.value = items.value.map((item)=>({
      ...item,
      isAdded: false
    }))
  })

    watch (filters, fetchItems)

</script>

<template>
    <div class="flex justify-between">
        <h2 class="text-3xl font-bold mb-8">All sneakers</h2>

        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none max-h-11" name="" id="">
            <option value="name">По названию</option>
            <option value="price">Сначала дешевые</option>
            <option value="-price">Сначала дорогие</option>
          </select>

        <div class="relative">
          <img class="absolute left-4 top-3" src="/search.svg" alt="search">
          <input 
            @input="onChangeSearchInput"
            type="text"
            placeholder="Поиск" 
            class="border rounded-md py-2 pl-11 pr-4 outline-none focus: border-gray-400"
          >
        </div>
        </div>
      </div>
      <div class="mt-10">
        <CardList :items="items" @addToFavorites="addToFavorites" @addToCart="onClickAddPlus"/>
      </div>
</template>