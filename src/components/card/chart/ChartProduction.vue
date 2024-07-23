<script setup>
import IconInfo from '@/components/icons/IconInfo.vue'
import data from '@/data/Production.json'
import { ref } from 'vue'

const series = ref(data.production.map((item) => item.value))
const labels = ref(data.production.map((item) => item.year))
const colors = ref(data.production.map((item) => item.color))

const chartOptions = ref({
  chart: {
    type: 'donut'
  },
  labels: labels.value,
  colors: colors.value,
  dataLabels: {
    enabled: false
  },
  legend: {
    show: false
  },
  responsive: [
    {
      breakpoint: 480,
      options: {
        chart: {
          width: 200
        }
      }
    }
  ]
})
</script>

<script>
import VueApexCharts from 'vue3-apexcharts'

export default {
  components: {
    apexchart: VueApexCharts
  }
}
</script>

<template>
  <div class="card">
    <div class="card-header">
      <h6 class="flex items-center gap-2">
        Overall by Production year
        <div class="flex items-center gap-4 relative tooltip">
          <IconInfo width="18" height="18" color="#b3b3b3" class="cursor-pointer" />
          <div
            class="absolute top-[50%] left-[40px] -translate-y-1/2 hidden transition-all bg-white text-black text-xs rounded-[8px] p-3 border border-[var(--neutral-light-100)] z-[2]"
          >
            <h6 class="text-[12px] mb-1">Production</h6>
            <p class="w-[fit]">Test tooltip</p>
          </div>
          <div class="arrow hidden"></div>
        </div>
      </h6>
    </div>
    <div class="card-body">
      <div class="relative p-0">
        <div id="chart-production">
          <apexchart type="donut" :options="chartOptions" :series="series"></apexchart>
          <div
            class="absolute top-[50%] left-[50%] -translate-x-1/2 -translate-y-1/2 z-10 flex flex-col items-center"
          >
            <span class="text-[28px] font-bold">{{ series[0] }}%</span>
            <span class="text-[16px]">{{ labels[0] }}</span>
          </div>
        </div>
      </div>
      <div class="chart-info grid grid-cols-2 gap-x-[48px] gap-y-6 p-[24px]">
        <div
          class="flex justify-between items-center"
          v-for="datum in data.production"
          :key="datum.id"
        >
          <div class="flex items-center gap-3">
            <div class="w-4 h-4 rounded-[4px]" :style="{ background: datum.color }"></div>
            <span>{{ datum.year }}</span>
          </div>
          <span class="font-bold">{{ datum.value }}%</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.chart-info > div:nth-child(2) {
  @apply order-3;
}

.chart-info > div:nth-child(3) {
  @apply order-2;
}

.chart-info > div:last-child {
  @apply order-4;
}
</style>
