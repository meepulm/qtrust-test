<script setup>
import IconExpand from '@/components/icons/IconExpand.vue'
import data from '@/data/ReplacementReasons.json'
</script>

<script>
import VueApexCharts from 'vue3-apexcharts'

export default {
  components: {
    apexchart: VueApexCharts
  },
  data() {
    const defaultYear = data.replacementReasons[0].year
    const defaultReasonsData = data.replacementReasons.find(
      (item) => item.year === defaultYear
    ).reason

    return {
      years: data.replacementReasons.map((item) => item.year),
      reasonsData: data.replacementReasons,
      selectedYear: defaultYear,
      series: defaultReasonsData.map((reason) => ({
        name: reason.name,
        data: reason.data,
        color: reason.color
      })),
      chartOptions: {
        chart: {
          type: 'bar',
          height: 350,
          stacked: true,
          toolbar: {
            show: false
          },
          zoom: {
            enabled: false
          }
        },
        dataLabels: {
          enabled: false
        },
        responsive: [
          {
            breakpoint: 480
          }
        ],
        plotOptions: {
          bar: {
            horizontal: false,
            borderRadius: 4,
            borderRadiusApplication: 'around',
            borderRadiusWhenStacked: 'all'
          }
        },
        xaxis: {
          categories: data.categories,
          labels: {
            style: {
              fontSize: '14px'
            }
          }
        },
        yaxis: {
          labels: {
            style: {
              fontSize: '14px'
            }
          }
        },
        legend: {
          show: true,
          fontSize: '15px',
          height: 48,
          markers: {
            radius: 4,
            offsetX: -6
          },
          itemMargin: {
            horizontal: 16,
            vertical: 0
          }
        },
        fill: {
          opacity: 1
        },
        tooltip: {
          shared: true,
          followCursor: true,
          intersect: false
        }
      }
    }
  },
  watch: {
    selectedYear() {
      this.updateSeries()
    }
  },
  methods: {
    updateSeries() {
      const selectedData = this.reasonsData.find((item) => item.year === this.selectedYear).reason
      this.series = selectedData.map((reason) => ({
        name: reason.name,
        data: reason.data,
        color: reason.color
      }))
    }
  }
}
</script>

<template>
  <div class="card">
    <div class="card-header !border-0 !pb-0 !px-[36px] !pt-[20px]">
      <h6 class="text-[18px]">Replacement Reasons</h6>
      <div class="relative">
        <select name="select-year" id="select-year" v-model="selectedYear">
          <option v-for="year in years" :key="year" :value="year">{{ year }}</option>
        </select>
        <IconExpand
          class="absolute top-[50%] right-[6px] -translate-y-1/2 bg-white"
          width="16px"
          height="16px"
        />
      </div>
    </div>
    <div class="card-body !pb-0">
      <div id="chart-replacement-reasons">
        <apexchart type="bar" height="350" :options="chartOptions" :series="series"></apexchart>
      </div>
    </div>
  </div>
</template>
