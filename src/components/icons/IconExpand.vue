<script>
export default {
  props: {
    width: { type: Number, default: 24 },
    height: { type: Number, default: 24 },
    color: { type: String, default: 'black' }
  }
}
</script>

<template>
  <svg
    :width="width"
    :height="height"
    viewBox="0 0 24 24"
    fill="none"
    xmlns="http://www.w3.org/2000/svg"
  >
    <path
      d="M13.5429 17.1293C12.7429 18.0992 11.2571 18.0992 10.4571 17.1293L6.03892 11.7726C4.963 10.4681 5.89089 8.5 7.58182 8.5H16.4182C18.1091 8.5 19.037 10.4681 17.9611 11.7726L13.5429 17.1293Z"
      :fill="color"
    />
  </svg>
</template>
