<script setup>
import AtomButton from '@/components/atom/AtomButton.vue'
import IconExpand from '@/components/icons/IconExpand.vue'
import data from '@/data/Segment.json'
</script>

<script>
import VueApexCharts from 'vue3-apexcharts'

export default {
  components: {
    apexchart: VueApexCharts
  },
  data() {
    const defaultYear = data.segment[0].year
    const defaultSegmentData = data.segment.find((segment) => segment.year === defaultYear).segment
    const defaultSegment = defaultSegmentData.find((segment) => segment.name === 'All Segment')

    return {
      years: data.segment.map((segment) => segment.year),
      segmentsData: data.segment,
      selectedYear: defaultYear,
      selectedSegment: defaultSegment.name,
      series: [
        {
          name: defaultSegment.name,
          data: defaultSegment.data
        }
      ],
      chartOptions: {
        chart: {
          height: 350,
          type: 'line',
          toolbar: {
            show: false
          }
        },
        colors: ['#55B586'],
        dataLabels: {
          enabled: false
        },
        stroke: {
          curve: 'straight'
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
        }
      }
    }
  },
  watch: {
    selectedYear() {
      this.updateSegments()
    },
    selectedSegment(newSegment) {
      this.updateSeries(newSegment)
    }
  },
  methods: {
    updateSegments() {
      const segments = this.segmentsData.find(
        (segment) => segment.year === this.selectedYear
      ).segment
      this.updateSeries(this.selectedSegment, segments)
    },
    updateSeries(segmentName, segments = null) {
      if (!segments) {
        segments = this.segmentsData.find((segment) => segment.year === this.selectedYear).segment
      }
      const selectedSegment = segments.find((segment) => segment.name === segmentName)
      this.series = [
        {
          name: selectedSegment.name,
          data: selectedSegment.data
        }
      ]
    },
    generateCSV() {
      const segments = this.segmentsData.find(
        (segment) => segment.year === this.selectedYear
      ).segment
      const selectedSegment = segments.find((segment) => segment.name === this.selectedSegment)
      const headers = ['Month', 'Data']
      const rows = data.categories.map((category, index) => [category, selectedSegment.data[index]])

      let csvContent =
        'data:text/csv;charset=utf-8,' +
        [headers.join(','), ...rows.map((e) => e.join(','))].join('\n')

      const encodedUri = encodeURI(csvContent)
      const link = document.createElement('a')
      link.setAttribute('href', encodedUri)
      link.setAttribute('download', `Segment - ${this.selectedYear} ${this.selectedSegment}.csv`)
      document.body.appendChild(link)
      link.click()
      document.body.removeChild(link)
    }
  }
}
</script>

<template>
  <div class="card">
    <div class="card-header !border-0 !pb-0 !px-[36px] !pt-[20px]">
      <h6 class="text-[18px]">Segment</h6>
      <div class="flex items-center gap-4">
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
        <div class="relative">
          <select name="select-segment" id="select-segment" v-model="selectedSegment">
            <option
              v-for="segment in segmentsData.find((segment) => segment.year === selectedYear)
                .segment"
              :key="segment.name"
              :value="segment.name"
            >
              {{ segment.name }}
            </option>
          </select>
          <IconExpand
            class="absolute top-[50%] right-[6px] -translate-y-1/2 bg-white"
            width="16px"
            height="16px"
          />
        </div>

        <AtomButton @click="generateCSV">Download CSV</AtomButton>
      </div>
    </div>
    <div class="card-body">
      <div id="chart-segment">
        <apexchart type="line" height="350" :options="chartOptions" :series="series"></apexchart>
      </div>
    </div>
  </div>
</template>
