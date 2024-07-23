<script setup>
import { RouterLink } from 'vue-router'
</script>

<script>
export default {
  props: {
    link: String
  }
}
</script>

<template>
  <RouterLink
    :to="link"
    activeClass="active !border-red-600"
    class="block border-l-[4px] border-white px-4 py-2"
  >
    <slot> </slot>
  </RouterLink>
</template>
