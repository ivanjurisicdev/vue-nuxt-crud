<script>
import { PolarArea } from 'vue-chartjs'
import axios from 'axios'

export default {
  extends: PolarArea,
  data: () => ({
    chartCategories: [],
    chartnumberOfProducts: [],
    chartdata: {
      labels: ['aaa', 'bbb'],

      datasets: [
        {
          data: [100, 200],
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
        },
      ],
    },
    options: {
      responsive: true,
      maintainAspectRatio: false,
      animation: {
        animateRotate: true,
        animateScale: true,
      },
    },
  }),
  created() {
    this.fetchProducts()
  },
  mounted() {
    this.renderChart(this.chartdata, this.options)
  },
  methods: {
    fetchProducts() {
      // console.log(this.chartCategories)
      // console.log(this.chartnumberOfProducts)
      // const categoriesApi =
      //   'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/stats/categories'
      axios
        .get(
          'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/stats/categories'
        )
        .then((response) => {
          // console.log(response.data)
          this.chartData = response.data
          this.chartCategories = this.chartData.map((x) => {
            for (const k in x) return x.category
          })
          // const fix1 = JSON.parse(JSON.stringify(this.chartCategories))
          // console.log(this.chartCategories)
          this.chartnumberOfProducts = this.chartData.map((x) => {
            for (const k in x) return x.numberOfProducts
          })
          // const fix2 = JSON.parse(JSON.stringify(this.chartnumberOfProducts))
          // console.log(this.chartnumberOfProducts)
        })
    },
  },
}
</script>
