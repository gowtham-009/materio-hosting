<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { Chart, registerables } from 'chart.js'

Chart.register(...registerables)

const lineChart = ref<HTMLCanvasElement | null>(null)

const data = {
  labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
  datasets: [{
    label: 'Looping tension',
    data: [65, 59, 80, 81, 26, 55, 40],
    fill: false,
    borderColor: 'rgb(75, 192, 192)',
  }],
}

const config = {
  type: 'line',
  data,
  options: {
    animations: {
      tension: {
        duration: 1000,
        easing: 'linear',
        from: 1,
        to: 0,
        loop: true,
      },
    },
    scales: {
      y: {
        min: 0,
        max: 100,
      },
    },
  },
}

onMounted(() => {
  if (lineChart.value)
    new Chart(lineChart.value.getContext('2d') as CanvasRenderingContext2D, config)
})
</script>

<template>
  <div>
    <canvas ref="lineChart" />
  </div>
</template>

<style scoped>
canvas {
  width: 100%;
  height: 400px;
}
</style>
