<script setup>
import { defineEmits } from 'vue'
import IconMenu from '@/components/icons/IconMenu.vue'

const emit = defineEmits(['toggle-sidebar'])

const handleClick = () => {
  emit('toggle-sidebar')
}
</script>

<template>
  <header
    id="header"
    class="fixed top-0 left-0 bg-white px-4 py-3 w-full border-b-[1px] border-[var(--neutral-light-100)] z-50 flex items-center justify-between"
  >
    <h1 class="text-black">Logo</h1>
    <div @click="handleClick" class="cursor-pointer block md:hidden">
      <IconMenu />
    </div>
  </header>
</template>
