<script setup>
import DrawerHead from './DrawerHead.vue';
import CartListItem from './CartListItem.vue';
import InfoBlock from './InfoBlock.vue'
import { inject, ref } from 'vue';
import axios from 'axios';

const { cart, toggleDrawer, tax, cartTotal } = inject('cart')
const isCreatingOrder = ref(false)
const orderId = ref(null)

const createOrder = async () => {
    try {
        isCreatingOrder.value = true
        const { data } = await axios.post('https://f0140d44a488f752.mokky.dev/order', {
            items: cart.value,
            totalPrice: cartTotal.value,
        })
        cart.value = []
        orderId.value = data.id

        return data
    } catch (err) {
        console.log(err)
    } finally {
        isCreatingOrder.value = false
    }
}
</script>

<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70" @click="toggleDrawer"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <DrawerHead />

        <div v-if="cartTotal">
            <CartListItem />
            <div class="flex flex-col gap-4 mt-7">
                <div class="flex gap-2 ">
                    <span>Итого:</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ cartTotal }} &#8381;</b>
                </div>
                <div class="flex gap-2 ">
                    <span>Налог 5%</span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>{{ tax }} &#8381;</b>
                </div>
                <button
                    class="bg-lime-500  rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 transition active:bg-lime-700 cursor-pointer my-7"
                    @click="createOrder" :disabled="cartTotal ? false : true">
                    Оформить заказ
                </button>
            </div>
        </div>
        <div v-if="orderId" class="flex h-full items-center">
            <InfoBlock title="Заказ оформлен!"
                :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
                image-url='./order-success-icon.png' @onClickBack="toggleDrawer" />
        </div>
        <div v-if="!cartTotal && !orderId" class="flex h-full items-center">
            <InfoBlock title="Корзина пустая" description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
                image-url='./package-icon.png' @onClickBack="toggleDrawer" />
        </div>

    </div>

</template>