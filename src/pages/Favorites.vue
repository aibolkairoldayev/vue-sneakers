<script setup>
    import axios from 'axios'
    import { ref, onMounted} from 'vue'

    import CardList from '../components/CardList.vue'

    const favorites = ref([])

    onMounted(async ()=>{
        try {
            const {data} = await axios.get('https://7687af431bcf909f.mokky.dev/favorites?_relations=items')

            favorites.value = data.map((obj)=> obj.item)
            console.log(favorites)
        } catch (err) {
            console.log(err)
        }
    })
</script>

<template>
    
    <h2 class="text-3xl font-bold mb-8">Мои Закладки</h2>

    <div class="mt-4">
        <CardList :items="favorites" is-favorites/>
    </div>
</template>