<script setup>
import AtomButton from '@/components/atom/AtomButton.vue'
import ChartProduction from '@/components/card/chart/ChartProduction.vue'
import IconDate from '@/components/icons/IconDate.vue'
import IconInfo from '@/components/icons/IconInfo.vue'
import IconExpand from '@/components/icons/IconExpand.vue'
import data from '@/data/Replacement.json'
</script>

<script>
import VueApexCharts from 'vue3-apexcharts'
import 'vue-datepicker-ui/lib/vuedatepickerui.css'
import VueDatepickerUi from 'vue-datepicker-ui'

export default {
  components: {
    apexchart: VueApexCharts,
    Datepicker: VueDatepickerUi
  },
  data() {
    return {
      selectedDate: [new Date(), new Date(new Date().getTime() + 9 * 24 * 60 * 60 * 1000)],

      years: ['All Year', ...data.replacement.map((item) => item.year)],
      selectedYear: 'All Year',
      series: data.replacement
        .map((item) => ({
          name: item.year,
          data: item.data,
          color: item.color
        }))
        .reverse(),
      chartOptions: {
        chart: {
          type: 'bar',
          height: 350,
          stacked: true,
          toolbar: {
            show: false
          },
          fontFamily: 'Montserrat'
        },
        dataLabels: {
          enabled: false
        },
        grid: {
          show: false
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
              fontSize: '16px'
            }
          }
        },
        yaxis: {
          labels: {
            style: {
              fontSize: '16px'
            }
          }
        },
        legend: {
          show: false
        },
        fill: {
          opacity: 1
        },
        tooltip: {
          shared: true,
          intersect: false,
          followCursor: true,
          inverseOrder: true,
          style: {
            fontSize: '14px'
          }
        }
      }
    }
  },
  watch: {
    selectedYear(newYear) {
      this.updateSeries(newYear)
    }
  },
  methods: {
    updateSeries(year) {
      if (year === 'All Year') {
        this.series = data.replacement
          .map((item) => ({
            name: item.year,
            data: item.data,
            color: item.color
          }))
          .reverse()
      } else {
        const selectedData = data.replacement.find((item) => item.year === year)
        this.series = [
          {
            name: selectedData.year,
            data: selectedData.data,
            color: selectedData.color
          }
        ]
      }
    },
    generateCSV() {
      const selectedData = data.replacement.find((item) => item.year === this.selectedYear)

      if (!selectedData) {
        const headers = ['Year', ...data.categories]
        const rows = data.replacement.map((item) => [item.year, ...item.data])

        let csvContent =
          'data:text/csv;charset=utf-8,' +
          [headers.join(','), ...rows.map((e) => e.join(','))].join('\n')

        const encodedUri = encodeURI(csvContent)
        const link = document.createElement('a')
        link.setAttribute('href', encodedUri)
        link.setAttribute('download', `Replacement - All Years.csv`)
        document.body.appendChild(link)
        link.click()
        document.body.removeChild(link)
      } else {
        const headers = ['Month', 'Data']
        const rows = data.categories.map((category, index) => [category, selectedData.data[index]])

        let csvContent =
          'data:text/csv;charset=utf-8,' +
          [headers.join(','), ...rows.map((e) => e.join(','))].join('\n')

        const encodedUri = encodeURI(csvContent)
        const link = document.createElement('a')
        link.setAttribute('href', encodedUri)
        link.setAttribute('download', `Replacement - ${selectedData.year}.csv`)
        document.body.appendChild(link)
        link.click()
        document.body.removeChild(link)
      }
    }
  }
}
</script>

<template>
  <div class="col-span-12 flex w-full items-center justify-between">
    <h5 class="font-bold text-[20px]">Replacement</h5>
    <div class="flex items-center gap-4">
      <div class="relative">
        <datepicker range v-model="selectedDate" lang="en" position="right" />
        <IconDate class="absolute top-[50%] left-[10px] -translate-y-1/2 bg-white" />
      </div>
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
      <AtomButton @click="generateCSV">Download CSV</AtomButton>
    </div>
  </div>
  <div class="col-span-12 md:col-span-9">
    <div class="card">
      <div class="card-header">
        <h6 class="flex items-center gap-2">
          Replacement
          <div class="flex items-center gap-4 relative tooltip">
            <IconInfo width="18" height="18" color="#b3b3b3" class="cursor-pointer" />
            <div
              class="absolute top-[50%] left-[40px] -translate-y-1/2 hidden transition-all bg-white text-black text-xs rounded-[8px] p-3 border-[1px] border-[var(--neutral-light-100)] z-[2]"
            >
              <h6 class="text-[12px] mb-1">Replacement</h6>
              <p class="w-[180px]">Kendaraan yang melakukan pergantian QR di H2</p>
            </div>
            <div class="arrow hidden"></div>
          </div>
        </h6>
      </div>
      <div class="card-body h-full">
        <div id="chart-replacement h-full">
          <apexchart type="bar" height="416" :options="chartOptions" :series="series"></apexchart>
        </div>
      </div>
    </div>
  </div>
  <div class="col-span-12 md:col-span-3">
    <ChartProduction />
  </div>
</template>
