<template>
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
        <tr
          v-for="c in cartList"
          :key="c.id"
        >
          <td>{{ c.name }}</td>
          <td>
            <input
              type="number"
              min="1"
              class="quantity"
              :value="c.qty">
            </td>
          <td>${{ c.price }}</td>
          <td>${{ c.price * c.qty }}</td>
          <td><button
              @click="removeFromCart(c.id)"
              :data-id="c.id"
              class="remove-item-btn btn btn-danger btn-sm"
            >
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
      @click="cleanCart"
      class="btn btn-lg btn-success empty-cart"><i class="fas fa-baby-carriage"></i> 清空購物車</button>
  </section>
</template>

<script>
import { computed } from 'vue';
export default {
  props: {
    cartList: Array
  },
  setup(props, context){
    
    // 從購物車移除
    const removeFromCart = id => {
      context.emit('remove', id);
    }
    // 清除購物車
    const cleanCart = () => {
      context.emit('remove-all');
    };
    // 計算總價
    const totalPrice = computed(() => Math.round(props.cartList.reduce((sum, item) => sum + item.price * item.qty, 0) * 100) / 100);
    return {
      removeFromCart,
      cleanCart,
      totalPrice
    }
  }
}
</script>
