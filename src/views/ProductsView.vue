<template>
  <div class="container">
    <div class="text-center my-4">
      <h2>產品列表</h2>
    </div>
    <div class="row row-cols-1 row-cols-lg-4 g-4">
      <div class="col" v-for="product in products" :key="product.id">
        <div class="card h-100">
          <img
            :src="product.imageUrl"
            class="card-img-top"
            alt="..."
            style="height: 200px"
          />
          <div class="card-body">
            <h5 class="card-title">{{ product.title }}</h5>
            <p class="card-text">
              {{ product.description }}
            </p>
          </div>
          <div class="card-footer d-flex justify-content-end">
            <router-link :to="`/product/${product.id}`" class="btn btn-primary me-1"
              >查看細節</router-link
            >
            <button
              type="button"
              class="btn btn-danger"
              @click="addToCart(product.id)"
            >
              加入購物車
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import emitter from '@/libs/emitter'
export default {
  data () {
    return {
      products: []
    }
  },
  methods: {
    getProducts () {
      this.$http
        .get(
          `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/products/all`
        )
        .then((res) => {
          this.products = res.data.products
        })
    },
    addToCart (id, qty = 1) {
      const data = {
        product_id: id,
        qty
      }
      this.$http.post(`${process.env.VUE_APP_API}/api/${process.env.VUE_APP_PATH}/cart`, { data }).then((res) => {
        emitter.emit('getCart')
        alert('已加入購物車')
      })
    }
  },
  mounted () {
    this.getProducts()
  }
}
</script>
