<template>
  <div>
    <h1>Reviews</h1>
    <p>Title: {{ $route.params.title }}</p>
    <p>Description: {{ $route.params.description }}</p>
    <p>Price: {{ $route.params.price }}</p>
    <p>Category: {{ $route.params.category }}</p>
  </div>
</template>

<script>
export default {
  props: ['id', 'title', 'description', 'price', 'category'],
}
</script>
