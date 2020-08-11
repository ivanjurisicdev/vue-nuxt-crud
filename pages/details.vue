<template>
  <div>
    <div v-if="$route.params.title" class="product-detail">
      <div class="container">
        <h1 class="main-title">
          {{ $route.params.title }} (
          <span>{{ $route.params.category }}</span>
          )
        </h1>

        <div class="image">
          <img
            src="https://images2.minutemediacdn.com/image/upload/c_crop,h_842,w_1500,x_0,y_88/f_auto,q_auto,w_1100/v1555006794/shape/mentalfloss/istock-522735736.jpg"
          />
        </div>
        <p>
          {{ $route.params.description }}...
          <span
            >There are many variations of passages of Lorem Ipsum available, but
            the majority have suffered alteration in some form, by injected
            humour, or randomised words which don't look even slightly
            believable. If you are going to use a passage of Lorem Ipsum, you
            need to be sure there isn't anything embarrassing hidden in the
            middle of text. All the Lorem Ipsum generators on the Internet tend
            to repeat predefined chunks as necessary, making this the first true
            generator on the Internet. It uses a dictionary of over 200 Latin
            words, combined with a handful of model sentence structures, to
            generate Lorem Ipsum which looks reasonable. The generated Lorem
            Ipsum is therefore always free from repetition, injected humour, or
            non-characteristic words etc.</span
          >
        </p>
        <p>
          You can get it for {{ $route.params.price }} €, visit our nearest
          store!
        </p>
      </div>
    </div>

    <h1 v-else class="empty">Go back to list</h1>
  </div>
</template>

<script>
export default {
  props: {
    title: { type: String, default: 'Some title' },
    description: { type: String, default: 'Some description' },
    price: { type: String, default: 'Some price' },
    category: { type: String, default: 'Some category' },
  },
}
</script>

<style scoped>
.container {
  max-width: 900px;
  margin: 0 auto;
}

.main-title {
  margin-bottom: 32px;
  text-align: center;
}

.image {
  position: relative;
}

.price span {
  position: absolute;
  top: 50%;
  transform: translate(-50%, -50%);
  left: 50%;
}

img {
  width: 100%;
  margin-bottom: 32px;
}

.empty {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
</style>
