<script setup>
    import DrawerHead from './DrawerHead.vue'
    import CartItemList from './CartItemList.vue'
    import InfoBlock from './InfoBlock.vue'

    defineProps({
        totalPrice: Number,
        buttonDisabled: Boolean
    })

    const emit = defineEmits(['createOrder'])
</script>

<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 ">            
        <DrawerHead/>

        <div v-if="!totalPrice"  class="h-full items-center flex">
            <InfoBlock 
                
                title="Карзина пуста" 
                description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
                imageUrl="/package-icon.png"    
            />
        </div>

        <div v-else>
            <CartItemList />

            <div class="flex flex-col gap-4 mt-7">
                <div class="flex gap-2">
                    <span>Итого:</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{totalPrice}} тг</b>
                </div>

                <div class="flex gap-2">
                    <span>Налог 5%</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ totalPrice * .05 }} тг</b>
                </div>

                <button 
                    :disabled="buttonDisabled" 
                    class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white transition 
                    hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-300 cursor-pointer"
                    @click="()=>emit('createOrder')"
                >Оформить заказ</button>
            </div>
        </div>
        
        
    </div>
</template>