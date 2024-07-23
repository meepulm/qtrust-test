<script setup>
import IconDownload from '@/components/icons/IconDownload.vue'

const emit = defineEmits(['download-csv'])
</script>

<template>
  <button
    @click="emit('download-csv')"
    class="flex items-center gap-2 bg-white text-black border border-[var(--neutral-light-100)] px-[16px] py-[8px] rounded-[6px] text-[14px]"
  >
    <span>Download CSV</span>
    <IconDownload />
  </button>
</template>
