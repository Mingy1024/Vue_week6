<template>
  <div class="container">
    <div class="text-center">
      <h2>購物車</h2>
    </div>
    <div class="text-end">
      <button
        class="btn btn-outline-danger"
        type="button"
        @click="removeEntireCart"
        :disabled="!cartData.carts.length"
      >
        清空購物車
      </button>
    </div>
    <table class="table align-middle text-center">
      <thead>
        <tr>
          <th></th>
          <th>圖片</th>
          <th>品名</th>
          <th style="width: 150px">數量/單位</th>
          <th>金額</th>
        </tr>
      </thead>
      <tbody>
        <template v-if="cartData.carts">
          <tr v-for="item in cartData.carts" :key="item.id">
            <td width="50px">
              <button
                type="button"
                class="btn btn-outline-danger btn-sm"
                @click="removeCartItem(item.id)"
              >
                x
              </button>
            </td>
            <td style="width: 200px">
              <div
                :style="{ backgroundImage: `url(${item.product.imageUrl})` }"
                style="
                  height: 100px;
                  background-size: cover;
                  background-position: center;
                "
              ></div>
            </td>
            <td>{{ item.product.title }}</td>
            <td>
              <div class="input-group input-group-sm">
                <div class="input-group mb-3">
                  <select
                    id=""
                    class="form-select"
                    v-model="item.qty"
                    @change="updateCartItem(item)"
                  >
                    <option
                      :value="num"
                      v-for="num in 50"
                      :key="`${num}+${item.id}`"
                    >
                      {{ num }}
                    </option>
                  </select>
                  <span class="input-group-text" id="basic-addon2">{{
                    item.product.unit
                  }}</span>
                </div>
              </div>
            </td>
            <td class="text-center">{{ item.total }}</td>
          </tr>
        </template>
      </tbody>
      <tfoot>
        <tr>
          <td></td>
          <td colspan="3" class="text-end">總計</td>
          <td class="text-center">{{ cartData.final_total }}</td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script>
import emitter from '@/libs/emitter'
export default {
  data () {
    return {
      cartData: {
        carts: []
      }
    }
  },
  methods: {
    getCart () {
      this.$http
        .get(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`)
        .then((res) => {
          this.cartData = res.data.data
        })
    },
    removeCartItem (id) {
      this.$http
        .delete(
          `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${id}`
        )
        .then((res) => {
          this.getCart()
          emitter.emit('getCart')
          alert('已刪除該品項')
        })
    },
    removeEntireCart () {
      this.$http
        .delete(
          `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/carts`
        )
        .then((res) => {
          this.getCart()
          emitter.emit('getCart')
          alert('已清空購物車')
        })
    },
    updateCartItem (item) {
      const data = {
        product_id: item.id,
        qty: item.qty
      }
      this.$http
        .put(
          `${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart/${item.id}`,
          { data }
        )
        .then((res) => {
          this.getCart()
        })
    }
  },
  mounted () {
    this.getCart()
  }
}
</script>
