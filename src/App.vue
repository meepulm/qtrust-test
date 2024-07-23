<script setup>
import { ref } from 'vue'
import { RouterView } from 'vue-router'
import LayoutHeader from '@/components/layout/LayoutHeader.vue'
import LayoutSidebar from '@/components/layout/LayoutSidebar.vue'

const isSidebarVisible = ref(false)

const toggleSidebar = () => {
  isSidebarVisible.value = !isSidebarVisible.value
}
</script>

<template>
  <section>
    <LayoutHeader @toggle-sidebar="toggleSidebar" />
    <LayoutSidebar :is-sidebar-visible="isSidebarVisible" />
    <main
      id="main"
      class="bg-[var(--neutral-frame-100)] mt-[48px] px-5 md:px-9 pt-5 md:pt-7 pb-[72px] flex-1 ml-0 md:ml-[58px] min-h-dvh"
    >
      <RouterView />
    </main>
  </section>
</template>

<style scoped></style>
