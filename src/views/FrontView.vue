<template>
  <FrontNavbar></FrontNavbar>
  <h1>目前位於前台頁面</h1>
  <router-view></router-view>
</template>

<script>
import FrontNavbar from '@/components/FrontNavbar.vue'

export default {
  components: {
    FrontNavbar
  }
}
</script>
