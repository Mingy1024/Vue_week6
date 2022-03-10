<template>
  <div class="container mt-5">
    <form class="row justify-content-center" @submit.prevent="signIn">
      <div class="col-md-6">
        <h1 class="h3 mb-3 font-weight-normal">後台登入</h1>
        <div class="mb-2">
          <label for="inputEmail" class="sr-only">Email address</label>
          <input
            type="email"
            id="inputEmail"
            class="form-control"
            placeholder="Email address"
            v-model="user.username"
            required
            autofocus
          />
        </div>
        <div class="mb-2">
          <label for="inputPassword" class="sr-only">Password</label>
          <input
            type="password"
            id="inputPassword"
            class="form-control"
            v-model="user.password"
            placeholder="Password"
            required
          />
        </div>
        <div class="text-end mt-4">
          <router-link
            to="/products"
            class="btn btn-lg btn-secondary btn-block me-2"
            type="button"
            >返回</router-link
          >
          <button class="btn btn-lg btn-primary btn-block" type="submit">
            登入
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data () {
    return {
      user: {}
    }
  },
  methods: {
    signIn () {
      this.$http
        .post(`${process.env.VUE_APP_API}admin/signin`, this.user)
        .then((res) => {
          const { token, expired } = res.data
          document.cookie = `Token=${token};expires=${new Date(expired)};`
          this.$router.push('/admin/products')
        })
        .catch(() => {
          alert('登入失敗，請重新輸入')
        })
    }
  }
}
</script>
