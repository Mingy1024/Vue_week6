<template>
  <div class="container text-center">
    <h2>單一產品細節</h2>
    <div class="card mb-3">
      <div>
        <img
          :src="product.imageUrl"
          class="card-img-top"
          alt="..."
          style="width: 300px; height: 300px; object-fit: cover"
        />
      </div>
      <div class="card-body">
        <h3 class="card-title">
          {{ product.title }}
          <span class="badge bg-warning text-dark">{{ product.category }}</span>
        </h3>
        <p class="card-text">
          <span class="fs-4">商品描述 :</span>
          <br />
          {{ product.description }}
        </p>
        <p class="card-text">
          <span class="fs-4">商品內容 :</span>
          <br />
          {{ product.content }}
        </p>
        <div class="d-flex justify-content-center">
          <p class="card-text text-danger fw-bolder me-2">
            {{ product.price }}
          </p>
          <p class="card-text text-secondary">
            <del>{{ product.origin_price }}</del>
          </p>
          <p>元 /{{ product.unit }}</p>
        </div>
      </div>
    </div>
    <template v-for="(pic, key) in product.imagesUrl" :key="key">
      <img :src="pic" alt="其他同類型產品" class="images m-2 img-fluid" v-if="pic" />
    </template>
  </div>
</template>

<script>
export default {
  data () {
    return {
      product: {}
    }
  },
  methods: {
    getProduct () {
      const { id } = this.$route.params
      this.$http
        .get(
          `${process.env.VUE_APP_API}api/${process.env.VUE_APP_PATH}/product/${id}`
        )
        .then((res) => {
          this.product = res.data.product
        })
    }
  },
  mounted () {
    this.getProduct()
  }
}
</script>
