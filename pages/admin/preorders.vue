<template>
  <div>
    <div>
      <div v-for="preorder in preorders" @click="preorder.detailShow = !preorder.detailShow" :key="preorder.id" class="preorder">
        <div class="detail-row">
          <div>
            <h2>{{ preorder.firstname }} {{ preorder.lastname }}</h2>
            <p>{{ preorder.email }}</p>
          </div>
          <h3>#{{ preorder.unique_code }}</h3>
        </div>
        <div class="products">
          <div v-for="product in preorder.products" :key="product.id">
            <transition>
<!--              <div>{{ product.pivot.color }}</div>-->
              <img :src="product.image" style="position: relative; z-index: 1; margin: 2%; user-select: none;" :style="{ backgroundColor: product.pivot?.color ? product.pivot?.color : '#fff' }" width="400px">
              <img
                id="symbol"
                ref="symbol"
                :src="product.pivot.symbol"
                width="100px"
                :style="{ top: product.pivot.symbol_pos.y + 'px', left: product.pivot.symbol_pos.x + 'px' }"
              >
            </transition>
<!--            <div>-->
<!--              <div>-->
<!--                <h2>{{ product.name }}</h2>-->
<!--                <p>{{ product.type.name }}</p>-->
<!--              </div>-->
<!--              <div v-show="product.show">-->
<!--                <p>Select color</p>-->
<!--                <div class="colors">-->
<!--                  <ColorRadioInput :color="product.color" />-->
<!--                </div>-->
<!--                <br>-->
<!--                <p>Size</p>-->
<!--                <div class="sizes">-->
<!--                  <SizeRadioInput :size="product.size" />-->
<!--                </div>-->
<!--              </div>-->
<!--              <div>-->
<!--                <h2>{{ product.price }} $</h2>-->
<!--              </div>-->
<!--            </div>-->
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'AdminPreordersPage',
  layout: 'AdminLayout',
  auth: true,
  data () {
    return {
      preorders: []
    }
  },
  async fetch () {
    this.preorders = await this.$axios.get('/preorders').then(res => res)
    this.preorders = this.preorders.data.data
    this.preorders.map((preorder) => {
      preorder.detailShow = false
      return preorder
    })
  }
}
</script>

<style scoped>
.preorder {
  margin: 2% auto;
  display: flex;
  gap: 10px;
  width: 80%;
  justify-content: space-between;
  border: 1px solid #cecece;
  border-radius: 20px;
  cursor: pointer;
  padding: 2%;
}

.detail-row {
  display: flex;
  gap: 10px;
  width: 100%;
  justify-content: space-between;
  cursor: pointer;
}

.detail {
  display: flex;
  gap: 10px;
  justify-content: space-between;
  cursor: pointer;
}
</style>
