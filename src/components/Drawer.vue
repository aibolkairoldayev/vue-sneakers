<script setup>
    import {ref, computed, inject} from 'vue'
    import axios  from 'axios'

    import DrawerHead from './DrawerHead.vue'
    import CartItemList from './CartItemList.vue'
    import InfoBlock from './InfoBlock.vue'


    const props = defineProps({
        totalPrice: Number,
    })

    const emit = defineEmits(['createOrder'])

    const isCreatingOrder = ref(false)
    const cartIsEmpty = computed(()=> cart.value.length===0)
    const orderId = ref(null)

    const cartButtonDisabled = computed(()=> isCreatingOrder.value || cartIsEmpty.value)

    const {cart, closeDrawer} = inject('cart')

    const createOrder = async ()=> {
    try{
      isCreatingOrder.value = true

      const {data} = await axios.post('https://7687af431bcf909f.mokky.dev/orders', {
        items: cart.value,
        totalPrice: props.totalPrice.value
      })

      cart.value=[]

        orderId.value = data.id
        } catch(err) {
            console.log(err)
        } finally {
            isCreatingOrder.value = true
        }
    }
</script>

<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 ">            
        <DrawerHead/>

        <div v-if="!totalPrice || orderId"  class="h-full items-center flex">
            <InfoBlock 
                v-if="!totalPrice && !orderId"
                title="Карзина пуста" 
                description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
                imageUrl="/package-icon.png"    
            />

            <InfoBlock 
                v-if="orderId"
                title="Заказ оформлен!" 
                :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
                imageUrl="/order-success-icon.png"    
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
                    :disabled="cartButtonDisabled" 
                    class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white transition 
                    hover:bg-lime-600 active:bg-lime-700 disabled:bg-slate-300 cursor-pointer"
                    @click="createOrder"
                >Оформить заказ</button>
            </div>
        </div>
        
        
    </div>
</template>