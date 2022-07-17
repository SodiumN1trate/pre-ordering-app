<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light mb-5 shadow-sm p-3 mb-5 bg-white rounded">
    <a class="navbar-brand">Shop</a>
    <div class="navbar-nav">
      <NuxtLink v-for="navigation in navigations" :key="navigation" class="nav-item nav-link" :class="{active: ($route.fullPath === navigation.link)}" :to="navigation.link">{{ navigation.name }}</NuxtLink>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'IndexPage',
  layout: 'DefaultLayout',
  data () {
    return {
      navigations: []
    }
  },
  async fetch () {
    this.navigations = await fetch(process.env.API_ADDRESS + '/navigations').then(res => res.json())
    this.navigations = this.navigations.data
  }
}
</script>
