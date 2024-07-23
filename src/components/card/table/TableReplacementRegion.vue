<script setup>
import { ref, computed } from 'vue'
import AtomButton from '@/components/atom/AtomButton.vue'
import IconExpand from '@/components/icons/IconExpand.vue'
import data from '@/data/ReplacementRegion.json'

const selectedYear = ref('All Year')
const selectedMD = ref('Semua MD')

const uniqueYears = computed(() => {
  const years = new Set(data.replacementRegion.flatMap((region) => region.data.map((d) => d.year)))
  return ['All Year', ...Array.from(years)]
})

const uniqueMDs = computed(() => {
  const mdNames = new Set(data.replacementRegion.map((region) => region.name))
  return ['Semua MD', ...Array.from(mdNames)]
})

const filteredData = computed(() => {
  return data.replacementRegion.filter((region) => {
    const matchesMD = selectedMD.value === 'Semua MD' || region.name === selectedMD.value
    return matchesMD
  })
})

const getColumnHeaders = () => {
  if (selectedYear.value === 'All Year') {
    return ['Region Name', ...uniqueYears.value.slice(1)]
  } else {
    return ['Region Name', selectedYear.value]
  }
}

const getRowData = (datum) => {
  if (selectedYear.value === 'All Year') {
    return datum.data.map((d) => d.value)
  } else {
    const yearData = datum.data.find((d) => d.year === selectedYear.value)
    return yearData ? [yearData.value] : ['-']
  }
}

const downloadCSV = () => {
  const headers = getColumnHeaders()
  const rows = filteredData.value.map((datum) => [datum.name, ...getRowData(datum)])
  let csvContent =
    'data:text/csv;charset=utf-8,' +
    headers.join(',') +
    '\n' +
    rows.map((e) => e.join(',')).join('\n')

  const encodedUri = encodeURI(csvContent)
  const link = document.createElement('a')
  link.setAttribute('href', encodedUri)
  link.setAttribute('download', 'Replacement Region.csv')
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
}
</script>

<script>
export default {
  data() {
    return data
  }
}
</script>

<template>
  <div class="card">
    <div class="card-header">
      <h6>Replacement ID by Region</h6>
      <div class="flex items-center gap-4">
        <div class="relative">
          <select v-model="selectedYear" name="select-year" id="select-year">
            <option v-for="year in uniqueYears" :key="year" :value="year">{{ year }}</option>
          </select>
          <IconExpand
            class="absolute top-[50%] right-[6px] -translate-y-1/2 bg-white"
            width="16px"
            height="16px"
          />
        </div>
        <div class="relative">
          <select v-model="selectedMD" name="select-md" id="select-md">
            <option v-for="md in uniqueMDs" :key="md" :value="md">{{ md }}</option>
          </select>
          <IconExpand
            class="absolute top-[50%] right-[6px] -translate-y-1/2 bg-white"
            width="16px"
            height="16px"
          />
        </div>

        <AtomButton @click="downloadCSV">Download CSV</AtomButton>
      </div>
    </div>
    <div class="card-body !p-0 overflow-x-scroll md:overflow-x-hidden">
      <table>
        <thead>
          <tr>
            <th v-for="header in getColumnHeaders()" :key="header">{{ header }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="datum in filteredData" :key="datum.id">
            <td>{{ datum.name }}</td>
            <td v-for="value in getRowData(datum)" :key="value">{{ value }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
