<template>
  <main>
    <header>
      <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container-fluid">
          <a class="navbar-brand" href="/"><i class="fas fa-gem"></i> 賺很大商店</a>
          <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav">
              <li class="nav-item">
                <a class="nav-link active" aria-current="page" href="#">商品</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">當日特價</a>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </header>

    <section class="container mt-4">
      <div class="items row">

        <div class="col-sm-2" v-for="(item, idx) in itemList">
          <div class="card" :data-product-id="item.id">
            <img :src="`images/${item.cover}`" src="images/cat001.jpg" class="card-img-top" alt="">
            <div class="card-body">
              <h5 class="card-title fs-6 fw-light">{{ idx + 1 }} {{ item.name }}</h5>
              <p class="price">${{ item.price }}</p>
              <button
                @click="addCart(item)"
                class="btn btn-sm btn-warning fw-light">
                <i class="fas fa-cat"></i></button>
            </div>
          </div>
        </div>

      </div>
      <hr>

      // 查看購物車狀態
      <!-- <pre> {{ cart }} </pre> -->
      <!-- <pre> {{ totalPrice }} </pre> -->

      <section class="cart">
        <h2>購物車</h2>
        <table class="table cart-item-table">
          <thead>
            <tr>
              <th scope="col">項目</th>
              <th scope="col">數量</th>
              <th scope="col">單價</th>
              <th scope="col">小計</th>
              <th scope="col"></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in cart">
              <td>{{ item.name }}</td>
              <td><input type="number" class="quantity" min="1" v-model="item.qty"></td>
              <td>${{ item.price }}</td>
              <td>${{ item.price * item.qty }}</td>
              <td>
                <button
                  @click="removeCart(item.id)"
                  class="remove-item-btn btn btn-danger btn-sm">
                  <i class="fas fa-trash-alt"></i></button>
              </td>
          </tr>
          </tbody>
          <tfoot>
            <tr>
              <td colspan="2"></td>
              <td>總價</td>
              <td><span class="total-price">${{ totalPrice }}</span></td>
              <td></td>
            </tr>
          </tfoot>
        </table>
        <button
          @click="removeAllItem"
          class="btn btn-lg btn-success empty-cart">
          <i class="fas fa-baby-carriage"></i> 清空購物車</button>
      </section>
    </section>
  </main>
</template>

<script>
import { computed, ref } from 'vue';

export default {
  setup() {
    const itemList = ref([]);
    fetch('./item.json')
      .then( res => res.json())
      // .then( d => itemList.value = itemList.value.concat(d) );
      .then( d => itemList.value = d );


      const cart = ref([]);

      const addCart = item => {
        const idx = cart.value.findIndex(d => item.id === d.id)

        if( idx > -1 ) {
          cart.value[idx].qty++;
        }
        else {
          cart.value.push({
            ...item,
            qty: 1
          });
        }; 
      };


      const totalPrice = computed(() => {
        return (
          Math.round(
            cart.value.reduce(
              (total, currentitem) => total + currentitem.price * currentitem.qty,
              0
            ) * 100
          ) / 100
        );
      });

      const removeCart = itemId => {
        cart.value = cart.value.filter(item => item.id !=== itemId)
      };

      const removeAllItem = () => {
        cart.value.length= 0;
      };


    return {
      itemList,
      cart,
      addCart,
      totalPrice,
      removeCart,
      removeAllItem
    }
  }
}
</script>

<style>

</style>
