<script setup>
import { ref, computed } from 'vue'
import AtomButton from '@/components/atom/AtomButton.vue'
import IconExpand from '@/components/icons/IconExpand.vue'
import data from '@/data/ReplacementDealer.json'

// State for the selected year and Dealer name
const selectedYear = ref('All Year')
const selectedDealer = ref('All Dealer')

// Get unique years from the data
const uniqueYears = computed(() => {
  const years = new Set(data.replacementDealer.flatMap((dealer) => dealer.data.map((d) => d.year)))
  return ['All Year', ...Array.from(years)]
})

// Get unique Dealer names from the data
const uniqueDealers = computed(() => {
  const dealerNames = new Set(data.replacementDealer.map((dealer) => dealer.name))
  return ['All Dealer', ...Array.from(dealerNames)]
})

// Filtered data based on the selected year and Dealer name
const filteredData = computed(() => {
  return data.replacementDealer.filter((dealer) => {
    const matchesDealer =
      selectedDealer.value === 'All Dealer' || dealer.name === selectedDealer.value
    return matchesDealer
  })
})

// Function to get the column headers based on the selected year
const getColumnHeaders = () => {
  if (selectedYear.value === 'All Year') {
    return ['Dealer Name', ...uniqueYears.value.slice(1)]
  } else {
    return ['Dealer Name', selectedYear.value]
  }
}

// Function to get the row data based on the selected year
const getRowData = (datum) => {
  if (selectedYear.value === 'All Year') {
    return datum.data.map((d) => d.value)
  } else {
    const yearData = datum.data.find((d) => d.year === selectedYear.value)
    return yearData ? [yearData.value] : ['-']
  }
}

// Function to download CSV
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
  link.setAttribute('download', 'Replacement Dealer.csv')
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
}
</script>

<template>
  <div class="card">
    <div class="card-header">
      <h6>Replacement ID by Dealer</h6>
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
          <select v-model="selectedDealer" name="select-dealer" id="select-dealer">
            <option v-for="dealer in uniqueDealers" :key="dealer" :value="dealer">
              {{ dealer }}
            </option>
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
