<script setup>
import { ref, onMounted } from 'vue'
import {
  mdiAccountMultiple,
  mdiCartOutline,
  mdiChartTimelineVariant,
  mdiReload,
} from '@mdi/js'
import * as chartConfig from '@/components/Charts/chart.config.js'
import LineChart from '@/components/Charts/LineChart.vue'
import SectionMain from '@/components/SectionMain.vue'
import CardBoxWidget from '@/components/CardBoxWidget.vue'
import CardBox from '@/components/CardBox.vue'
import TableSampleClients from '@/components/TableSampleClients.vue'
import NotificationBar from '@/components/NotificationBar.vue'
import BaseButton from '@/components/BaseButton.vue'
import LayoutAuthenticated from '@/layouts/LayoutAuthenticated.vue'
import SectionTitleLineWithButton from '@/components/SectionTitleLineWithButton.vue'

const chartData = ref(null)

const fillChartData = () => {
  chartData.value = chartConfig.sampleChartData()
}

onMounted(() => {
  fillChartData()
})

</script>

<template>
  <LayoutAuthenticated>
    <SectionMain>

      <SectionTitleLineWithButton  :icon="mdiChartTimelineVariant" title="Overview">
        <BaseButton :icon="mdiReload" color="whiteDark" @click="fillChartData" />
      </SectionTitleLineWithButton>

      <CardBox class="mb-6">
        <div v-if="chartData">
          <line-chart :data="chartData" class="h-96" />
        </div>
      </CardBox>
      <div class="grid grid-cols-1 gap-6 lg:grid-cols-3 mb-6">
        <CardBoxWidget
trend="12%" trend-type="up" color="text-emerald-500" :icon="mdiAccountMultiple" :number="512"
          label="Knowledge" suffix="pts"/>
        <CardBoxWidget
trend="12%" trend-type="down" color="text-blue-500" :icon="mdiCartOutline" :number="80"
          suffix="min" label="Wasted Time" />
        <CardBoxWidget
trend="Overflow" trend-type="info" color="text-red-500" :icon="mdiChartTimelineVariant"
          :number="256" suffix="%" label="Performance" />
      </div>


      <SectionTitleLineWithButton :icon="mdiAccountMultiple" title="Notes" />

      <NotificationBar color="info">
        Your Notes
      </NotificationBar>

      <CardBox has-table>
        <TableSampleClients />
      </CardBox>

    </SectionMain>
  </LayoutAuthenticated>
</template>
