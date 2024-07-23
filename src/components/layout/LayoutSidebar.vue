<script setup>
import AtomSideIcon from '@/components/atom/AtomSideIcon.vue'
import IconDashboard from '@/components/icons/IconDashboard.vue'
import IconFile from '@/components/icons/IconFile.vue'

const props = defineProps({
  isSidebarVisible: Boolean
})
</script>

<template>
  <div
    id="sidebar"
    :class="{ 'right-0': props.isSidebarVisible, 'right-[-100%]': !props.isSidebarVisible }"
    class="fixed top-[48px] left-[unset] md:left-0 md:right-[unset] w-full md:w-fit bg-black/50 md:bg-transparent z-40 transition-all flex justify-end md:justify-start"
  >
    <div class="bg-white pt-2 w-fit h-dvh -scale-x-100 md:scale-x-100">
      <AtomSideIcon link="/">
        <IconDashboard />
      </AtomSideIcon>
      <AtomSideIcon link="/about">
        <IconFile />
      </AtomSideIcon>
    </div>
  </div>
</template>
