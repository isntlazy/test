<template lang="pug">
  v-container
    v-row
      v-col(cols)
        DataTable(
          v-if="items.length"
          :headers="headers"
          :items="formattedItems"
        )
        v-progress-circular(
          v-else
          width="2"
          color="rs__primary"
          indeterminate
        ).mx-auto
</template>

<script>
import DataTable from '~/components/DataTable.vue'
import sales from '~/api/sales.js'

export default {
  components: {
    DataTable
  },
  data() {
    return {
      sales,
      items: [],
      headers: [
        { text: 'Title', value: 'user.title', align: 'start', sortable: false },
        { text: 'Full Name', value: 'userFullName', align: 'start', sortable: false, },
        { text: 'Email', value: 'email', align: 'start', sortable: false },
        { text: 'Gender', value: 'gender', align: 'center', sortable: false },
        { text: 'Year', value: 'year', align: 'center', sortable: false },
        { text: 'Sales', value: 'sales', align: 'center', sortable: false },
        { text: 'Country', value: 'country', align: 'center', sortable: false, },
      ],
    }
  },
  computed: {
    formattedItems() {
      return this.items.map(item => {
        item.userFullName = `${item.user.first_name} ${item.user.last_name}`
        return item
      })
    }
  },
  async created() {
    this.items = await this.fetchData(0, 50)
  },
  methods: {
    async fetchData(page, size) {
      const start = page * size
      await this.delay(3000)
      return await sales.results.slice(start, start + size)
    },
    delay(ms) {
      return new Promise(resolve => setTimeout(resolve, ms))
    }
  }
}
</script>

<style lang="sass" scoped>
.v-progress-circular
  position: absolute
  top: 50%
  left: 50%
</style>
