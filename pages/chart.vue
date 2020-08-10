<template>
  <div>
    <!-- <div v-for="product in chartData" :key="product.id" class="product">
      <span>{{ product.category }}</span>
      <span>{{ product.numberOfProducts }}</span>
      <br />
    </div> -->

    <br /><br />

    <canvas id="my-chart" width="500" height="300"></canvas>
  </div>
</template>

<script>
import Chart from 'chart.js'
import axios from 'axios'
export default {
  /* eslint no-new:0 */
  data: () => ({
    // chartData: [
    //   { category: 'Category 1', numberOfProducts: 100 },
    //   { category: 'Category 2', numberOfProducts: 1200 },
    //   { category: 'Category 3', numberOfProducts: 3300 },
    // ],
    chartData: [],
    chartCategories: null,
    chartnumberOfProducts: null,
  }),
  mounted() {
    new Chart(document.getElementById('my-chart'), {
      type: 'polarArea',
      data: {
        labels: ['aaa', 'bbb'],
        datasets: [
          {
            data: [100, 200], // Specify the data values array
            borderColor: [
              '#2196f38c',
              '#f443368c',
              '#3f51b570',
              '#00968896',
              '#ffffff',
            ], // Add custom color border (Line)
            backgroundColor: [
              '#2196f38c',
              '#f443368c',
              '#3f51b570',
              '#00968896',
              '#ffffff',
            ], // Add custom color background (Points and Fill)
            borderWidth: 1, // Specify bar border width
          },
        ],
      },
      options: {
        responsive: true, // Instruct chart js to respond nicely.
        maintainAspectRatio: false, // Add to prevent default behaviour of full-width/height
        animation: {
          animateRotate: true,
          animateScale: true,
        },
      },
    })
    // console.log(this.chartData)
    // console.log(this.chartData[0].category)
    // console.log(this.chartData[0].numberOfProducts)
    // this.chartCategories = this.chartData.map((x) => {
    //   for (const k in x) return x.category
    // })
    // console.log(this.chartCategories)
    // this.chartnumberOfProducts = this.chartData.map((x) => {
    //   for (const k in x) return x.numberOfProducts
    // })
    // console.log(this.chartnumberOfProducts)
  },
  created() {
    this.fetchProducts()
  },
  methods: {
    fetchProducts() {
      // console.log(this.chartCategories)
      // console.log(this.chartnumberOfProducts)
      const categoriesApi =
        'http://us-central1-test-b7665.cloudfunctions.net/api/stores/ijpxNJLM732vm8AeajMR/stats/categories'
      axios
        .get(categoriesApi)
        .then((response) => {
          console.log(response.data)
          this.chartData = response.data
          this.chartCategories = this.chartData.map((x) => {
            for (const k in x) return x.category
          })
          const fix1 = JSON.parse(JSON.stringify(this.chartCategories))
          console.log(fix1)
          this.chartnumberOfProducts = this.chartData.map((x) => {
            for (const k in x) return x.numberOfProducts
          })
          const fix2 = JSON.parse(JSON.stringify(this.chartnumberOfProducts))
          console.log(fix2)
        })
        .catch(function (error) {
          console.log(error.response)
        })
    },
  },
}
</script>

<style scoped></style>
