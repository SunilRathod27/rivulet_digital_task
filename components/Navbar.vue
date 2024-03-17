<template>
  <div>
    <nav class="header">
      <nuxt-link to="/"><span>Home</span></nuxt-link>
      <nuxt-link to="/product"><span>Create Product</span></nuxt-link>
      <nuxt-link to="/category"><span>Create Category</span></nuxt-link>
      <nuxt-link to="/subcategory"><span></span>Create Sub Category</nuxt-link>

      <nuxt-link v-if="$auth.$state.loggedIn" to="/profile">
        <span>Hello, {{ $auth.$state.user.name }} </span>
      </nuxt-link>

      <nuxt-link v-else to="/signup">
        <span>Hello, Signin</span>
      </nuxt-link>

      <nuxt-link to="/cart"><span>Cart - {{ getCartLength }} </span></nuxt-link>
    </nav>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
export default {
  computed : {
    ...mapGetters(['getCartLength'])
  }
};
</script>

<style scoped>
.header {
  width: 100%;
  height: 70px;
  background-color: black;
  color: white;
  display: flex;
  align-items: center;
  justify-content: space-around;
}
</style>