<template>
  <nav
    class="navbar navbar-expand-lg navbar-light"
    style="background-color: #20c997"
  >
    <div class="container-fluid">
      <router-link class="navbar-brand" to="/">首頁</router-link>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <li class="nav-item">
            <router-link class="nav-link" to="/products">產品列表</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/cart">購物車</router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/admin">後台</router-link>
          </li>
        </ul>
        <router-link :to="`/cart`" class="btn btn-warning position-relative">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-bag-heart-fill"
            viewBox="0 0 16 16"
          >
            <path
              d="M11.5 4v-.5a3.5 3.5 0 1 0-7 0V4H1v10a2 2 0 0 0 2 2h10a2 2 0 0 0 2-2V4h-3.5ZM8 1a2.5 2.5 0 0 1 2.5 2.5V4h-5v-.5A2.5 2.5 0 0 1 8 1Zm0 6.993c1.664-1.711 5.825 1.283 0 5.132-5.825-3.85-1.664-6.843 0-5.132Z"
            />
          </svg>
          <span
            class="position-absolute top-0 start-100 translate-middle badge rounded-pill bg-danger"
            >{{ cartData.carts.length }}</span
          ></router-link
        >
      </div>
    </div>
  </nav>
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
    }
  },
  mounted () {
    this.getCart()
    emitter.on('getCart', () => {
      this.getCart()
    })
  }
}
</script>
