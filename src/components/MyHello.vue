<script setup>
import { ref } from 'vue'
const props = defineProps({
  title: String,
  price: Number,
  isAdded: Boolean
})
// export default {
//   data() {
//     return {
//       count: 0
//     }
//   },

//   methods: {
//     increment() {
//       this.count++
//     }
//   }
// }

const count = ref(0)

const increment = () => {
  count.value++
}

const updateValue = (e) => {
  count.value = Number(e.target.value)
}
</script>

<template>
  <h1>{{ count }}</h1>
  <button @click="increment">+</button>
  <input @input="updateValue" />
  <input v-model="count" />

  <div class="product">
    <h2>Товар: {{ props.title }}</h2>
    <h3>Цена: {{ props.price }}</h3>
    <h3>Добавлен {{ props.isAdded }}</h3>
  </div>
</template>

<style scoped>
.product {
  border: 1px solid blue;
  padding: 10px;
  margin-bottom: 10px;
}
</style>
