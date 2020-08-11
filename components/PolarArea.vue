<script>
import { PolarArea, mixins } from 'vue-chartjs'

import axios from 'axios'

export default {
  extends: PolarArea,
  mixins: [mixins.reactiveData],
  data() {
    return {
      chartData: '',
    }
  },
  mounted() {
    this.renderChart(this.chartData)
  },
  created() {
    axios
      .get(
        'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/stats/categories'
      )
      .then((response) => {
        const responseData = response.data
        this.chartData = {
          labels: responseData.map((item) => item.category),
          datasets: [
            {
              data: responseData.map((item) => item.numberOfProducts),
              borderColor: [
                '#2196f38c',
                '#f443368c',
                '#3f51b570',
                '#00968896',
                '#ffffff',
              ],
              backgroundColor: [
                '#2196f38c',
                '#f443368c',
                '#3f51b570',
                '#00968896',
                '#ffffff',
              ],
              borderWidth: 1,
              // options: {
              //   responsive: true,
              //   maintainAspectRatio: false,
              //   animation: {
              //     animateRotate: true,
              //     animateScale: true,
              //   },
              // },
            },
          ],
        }
      })
  },
}
</script>

<style scoped>
canvas {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
</style>
