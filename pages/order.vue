
<template>
  <div>
    <SucessMessage :success="success" />
    <div id="container" class="shadow-sm">
      <div id="preview" @drop="drop($event)" @dragover="$event.preventDefault()">
        <img :src="accessory.image" style="position: relative; z-index: 1; margin: 2%; user-select: none;" :style="{ backgroundColor: form.color?.color ? form.color.color : '#00' }" width="400px">
        <img
          id="symbol"
          ref="symbol"
          :style="{ top: form.symbol_pos.y + 'px', left: form.symbol_pos.x + 'px' }"
          :src="symbol.image"
          width="100px"
          draggable="true"
          @dragenter="dragEnter"
          @dragend="dragEnd"
          @dragstart="drag"
        >
      </div>
      <div id="detail">
        <div>
          <h2>{{ accessory.name }}</h2>
          <p>{{ accessory.type.name }}</p>
        </div>
        <div>
          <p :class="{ error: colorError}">Select color</p>
          <div id="colors">
            <ColorRadioInput v-for="color in colors" :key="color.id" :checked="true" :color="color" @input="colorOn(color.id)" />
          </div>
          <br>
          <p :class="{ error: sizeError}">Select size</p>
          <div id="sizes">
            <SizeRadioInput v-for="size in sizes" :key="size.id" v-model="form.size" :size="size" />
          </div>
        </div>
        <div>
          <h2>{{ accessory.price }} $</h2>
          <button type="submit" class="btn btn-primary" @click="submit">Add to cart</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'OrderPage',
  layout: 'DefaultLayout',
  auth: false,
  data () {
    return {
      symbol: this.$store.state.product.symbol,
      accessory: this.$store.state.product.accessory,
      colors: [],
      sizes: [],
      colorError: false,
      sizeError: false,
      form: {
        symbol: null,
        accessory: null,
        color: null,
        size: null,
        symbol_pos: {
          x: 156,
          y: 142
        }
      },
      success: null
    }
  },
  beforeMount () {
    if (this.accessory === null) {
      this.$router.push('/')
    }
  },
  async fetch () {
    this.colors = await this.$axios.get('/colors?type=' + this.accessory.type.id).then(res => res)
    this.form.color = this.colors.data.data[0]
    this.colors = this.colors.data.data.reverse()
    this.sizes = await this.$axios.get('/sizes?type=' + this.accessory.type.id).then(res => res)
    this.sizes = this.sizes.data.data
  },
  methods: {
    colorOn (colorId) {
      this.form.color = this.colors.filter(function (color) {
        return parseInt(colorId) === color.id
      })[0]
    },
    drop (event) {
      if (event.offsetX === event.layerX && event.offsetY === event.layerY) {
        this.form.symbol_pos.x = event.offsetX
        this.form.symbol_pos.y = event.offsetY
        this.$refs.symbol.style.left = event.offsetX + 'px'
        this.$refs.symbol.style.top = event.offsetY + 'px'
      }
    },
    drag (event) {
      event.target.style.border = '4px dotted gray'
      event.target.style.opacity = '0.6'
    },
    dragEnter (event) {
      event.target.style.border = '1px solid gray'
      event.target.style.opacity = '0.4'
    },
    dragEnd () {
      this.$refs.symbol.style.opacity = '1'
      this.$refs.symbol.style.border = 'none'
    },
    submit () {
      this.form.symbol = this.symbol.id
      this.form.accessory = this.accessory.id
      if (this.form.size === null) {
        this.sizeError = true
      } else {
        this.form.size = parseInt(this.form.size)
        this.$store.commit('addCart', this.form)
        this.success = { data: 'Added to cart! After 4 seconds you will be redirected to cart.' }
        this.$nextTick(() => {
          this.$nuxt.$loading.start()
          setTimeout(() => {
            this.$router.push('/cart')
            this.$nuxt.$loading.finish()
          }, 4000)
        })
      }
    }
  }
}
</script>

<style scoped>
#container {
  display: flex;
  flex-wrap: wrap;
  margin: auto;
  width: 80%;
  border: 1px solid #cecece;
  border-radius: 20px;
}
#preview {
  position: relative;
  width: 400px;
}

#symbol {
  position: absolute;
  transform: translateY(-50%);
  display: block;
  z-index: 2;
}

h2 {
  color: #575757;
}

#detail {
  display: flex;
  gap: 10px;
  justify-content: space-between;
  flex-direction: column;
  margin: 2%;
}

#colors, #sizes {
  display: flex;
  gap: 10px;
}

.error {
  color: red;
}
</style>
