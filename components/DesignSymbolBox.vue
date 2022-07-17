<template>
  <div class="item-box border shadow p-3 mb-5 bg-white rounded">
    <img class="shadow-sm p-3 mb-5 bg-white rounded" :src="image">
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  layout: 'DefaultLayout',
  props: ['image']
}
</script>

<style scoped>
.item-box {
  display: flex;
  flex-direction: column;
  transition: all .2s ease-in-out;
  width: 20%;
  cursor: pointer;
}

.item-box:hover {
  transform: scale(1.05);
}

</style>
