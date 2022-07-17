<template>
  <div>
    <h3 class="text-center mb-5">Choose a design symbol and put it on your favorite accessory or t-shirt!</h3>
    <div class="content">
      <DesignSymbolBox v-for="item in items" :key="item" :image="item.image"/>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  layout: 'DefaultLayout',
  auth: false,
  data () {
    return {
      items: []
    }
  },
  async fetch () {
    this.items = await fetch(process.env.API_ADDRESS + '/symbols').then(res => res.json())
    this.items = this.items.data
  }
}
</script>

<style scoped>
.content {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}
</style>
