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
      <ItemList :itemList="itemList" @add="addToCart" />
      <hr>

      <CartList :cartList="cartList"
        @remove="removeFromCart"
        @remove-all="cleanCart"
        />
    </section>
  </main>
</template>

<script>
import { ref } from 'vue';
import ItemList from './components/item.vue';
import CartList from './components/cart.vue';
export default {
  components: {
    ItemList,
    CartList
  },
  setup() {
    // 商品列表
    const itemList = ref([]);
    fetch('./item.json')
      .then(res => res.json())
      .then(d => itemList.value = itemList.value.concat(d));
    // 購物車列表
    const cartList = ref([]);

    // 加入購物車
    const addToCart = (item) => {
      const idx = cartList.value.findIndex((d) => item.id === d.id);
      if (idx > -1) {
        cartList.value[idx].qty++;
      } else {
        cartList.value.push({
          ...item,
          qty: 1,
        });
      }
    };
    // 從購物車移除
    const removeFromCart = (itemId) => {
      cartList.value = cartList.value.filter((item) => item.id != itemId);
    };
    // 清除購物車
    const cleanCart = () => {
      cartList.value.length = 0
    };
    return {
      itemList,
      cartList,
      addToCart,
      removeFromCart,
      cleanCart,
    }
  }
}
</script>
