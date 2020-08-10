<template>
  <div>
    <products />
    <polar-area />
    <chart-container />
  </div>
</template>

<script>
import Products from '~/components/Products.vue'
import PolarArea from '~/components/PolarArea.vue'
import ChartContainer from '~/components/ChartContainer'

export default {
  components: {
    Products,
    PolarArea,
    ChartContainer,
  },
}
</script>
