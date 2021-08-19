<template>
<div class="items row">
  <div
    class="col-sm-2"
    v-for="item in itemList"
    :key="item.id">
    <div
      :data-product-id="item.id"
      class="card"
    >
      <img
        :src="`./images/${item.cover}`"
        :alt="item.name"
        class="card-img-top"
      >
      <div class="card-body">
        <h5 class="card-title fs-6 fw-light">{{ item.name }}</h5>
        <p class="price">${{ item.price }}</p>
        <button
          @click="addToCart(item)"
          class="btn btn-sm btn-warning fw-light"
        ><i class="fas fa-cat"></i></button>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  props: {
    itemList: Array
  },
  setup(props, context) {
    const addToCart = item => {
      context.emit('add', item);
    }
    return {
      addToCart
    }
  }
}
</script>
