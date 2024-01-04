<script setup>
// import { inject } from 'vue'
import Card from './Card.vue'

defineProps({
  items: Array,
  isFavorites: Boolean
  // addToFavorite: Function
})

const emit = defineEmits(['addToFavorite', 'addToCart'])

// const addToFavorite = inject('addToFavorite')
</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :image-url="item.imageUrl"
      :price="item.price"
      :onClickFavorite="() => (isFavorites ? null : emit('addToFavorite', item))"
      :onClickAdd="() => (isFavorites ? null : emit('addToCart', item))"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
    />
  </div>
</template>
