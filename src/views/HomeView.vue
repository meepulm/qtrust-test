<script setup>
import ChartReplacement from '@/components/card/chart/ChartReplacement.vue'
import ChartSegment from '@/components/card/chart/ChartSegment.vue'
import ChartReplacementReasons from '@/components/card/chart/ChartReplacementReasons.vue'
import TableReplacementRegion from '@/components/card/table/TableReplacementRegion.vue'
import TableReplacementDealer from '@/components/card/table/TableReplacementDealer.vue'
</script>

<template>
  <div class="grid grid-cols-12 gap-4">
    <ChartReplacement />
    <div class="col-span-12">
      <ChartSegment />
    </div>
    <div class="col-span-12">
      <ChartReplacementReasons />
    </div>
    <div class="col-span-12">
      <TableReplacementRegion />
    </div>
    <div class="col-span-12">
      <TableReplacementDealer />
    </div>
  </div>
</template>
