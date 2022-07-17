<template>
  <div class="container">
    <div>
      <h1>Navigation</h1>
      <div class="under-content">
        <draggable @change="saveNavigation" :list="list" class="navigation-buttons d-flex flex-column">
          <div v-for="navigation in navigations" :key="navigation" class="card">
            <div class="card-body">
              {{ navigation.name }}
            </div>
          </div>
        </draggable>
        <button @click="saveNavigation" type="submit" class="btn btn-primary">Save</button>
      </div>
    </div>
  </div>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  name: 'IndexPage',
  components: {
    draggable
  },
  layout: 'DefaultLayout',
  auth: true,
  data () {
    return {
      navigations: [],
      list: null
    }
  },
  async fetch () {
    this.navigations = await fetch(process.env.API_ADDRESS + '/navigations').then(res => res.json())
    this.navigations = this.navigations.data
  },
  methods: {
    saveNavigation (e) {
      console.log(e)
    }
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

.under-content {
  margin-left: 2%;
}

.under-content > div {
  gap: 10px;
}

.under-content > button {
  margin-top: 1%;
}
</style>
