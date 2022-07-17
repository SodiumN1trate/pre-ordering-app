<template>
  <div class="item-box border shadow p-3 mb-5 bg-white rounded">
    <img class="shadow-sm p-3 mb-5 bg-white rounded" :src="image">
    <h5>{{ name }}</h5>
    <p>{{ type.name }}</p>
    <h4>{{ price }}$</h4>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  layout: 'DefaultLayout',
  props: ['name', 'image', 'price', 'type']
}
</script>

<style scoped>
.item-box {
  display: flex;
  flex-direction: column;
  transition: all .2s ease-in-out;
  cursor: pointer;
}

.item-box:hover {
  transform: scale(1.05);
}

</style>
