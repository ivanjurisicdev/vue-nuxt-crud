<template>
  <div class="container">
    <line-chart v-if="loaded" :chartdata="chartdata" :options="options" />
  </div>
</template>

<script>
import axios from 'axios'
import LineChart from './Chart.vue'

export default {
  name: 'LineChartContainer',
  components: { LineChart },
  data: () => ({
    loaded: false,
    chartdata: null,
    options: {
      responsive: true,
      maintainAspectRatio: false,
      animation: {
        animateRotate: true,
        animateScale: true,
      },
    },
  }),
  async mounted() {
    this.loaded = false
    try {
      await axios
        .get(
          'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/stats/categories'
        )
        .then(
          (response) =>
            (this.chartdata = response.data.map((x) => {
              for (const k in x) return x.category
            }))
        )

      this.loaded = true
    } catch (e) {
      console.error(e)
    }

    console.log('ovo me zanima')
    const fix1 = JSON.parse(JSON.stringify(this.chartdata))
    console.log(fix1)
    console.log(this.chartdata)
  },
}
</script>
