<script setup>
import { ref, inject, computed } from 'vue'
import axios from 'axios'
import CartItemList from './CartItemList.vue'
import Drawerhead from './Drawerhead.vue'
import InfoBlock from './InfoBlock.vue'

const { cart, closeDrawer } = inject('cart')
const isCreating = ref(false)
const orderId = ref(null)

const cartIsEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post(`https://6da82525633a209c.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })

    cart.value = []
    orderId.value = data.id
    return data
  } catch (error) {
    console.log(error)
  } finally {
    isCreating.value = false
  }
}
</script>

<template>
  <div class="fixed z-10 top-0 h-full w-full bg-black opacity-70"></div>
  <div
    class="flex flex-col justify-between fixed h-full z-10 top-0 right-0 w-96 bg-white px-10 py-7"
  >
    <Drawerhead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        image-url="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен!"
        :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
        image-url="/order-success-icon.png"
      />
    </div>

    <div v-else>
      <CartItemList />

      <div v-if="totalPrice" class="flex flex-col gap-4 my-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed" />
          <span class="font-bold">{{ totalPrice }} руб.</span>
        </div>

        <div class="flex items-end gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <span class="font-bold">{{ vatPrice }} руб.</span>
        </div>

        <button
          @click="createOrder"
          :disabled="buttonDisabled"
          class="flex justify-center items-center gap-3 w-full py-3 mt-10 bg-lime-500 text-white disabled:bg-slate-300 rounded-xl transition active:bg-lime-700 hover:bg-lime-600 cursor-pointer"
        >
          Оформить заказ
          <img src="/arrow-next.svg" alt="Arrow" />
        </button>
      </div>
    </div>
  </div>
</template>
