<template>
  <div class="content">
    <h3 class="text-center mb-5">Choose an accessory and put one of the design symbols on it!</h3>
    <div
      v-for="item in items"
      :key="item">
      <ProductBox
        v-if="item.type.id === 2"
        :image="item.image"
        :name="item.name"
        :type="item.type"
        :price="item.price"/>
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
    this.items = await fetch(process.env.API_ADDRESS + '/products').then(res => res.json())
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
