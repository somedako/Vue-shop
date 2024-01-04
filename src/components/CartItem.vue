<script setup>
const emit = defineEmits('onClickRemove')
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number
})
</script>

<template>
  <div class="flex items-center border border-slate-200 rounded-xl p-4 gap-4">
    <img :src="imageUrl" class="w-16 h-16" :alt="title" />
    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between mt-2">
        <span class="font-bold flex-1">{{ price }} руб.</span>
        <img
          @click="emit('onClickRemove')"
          class="cursor-pointer opacity-40 hover:opacity-100 transition"
          src="/close.svg"
          alt="Close"
        />
      </div>
    </div>
  </div>
</template>
