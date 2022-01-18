<template lang="pug">
  v-container
    v-row
      v-col(cols)
        DataTable(
          :headers="headers"
          :items="formattedItems"
          :loading="loading"
          :totalItemsCount="sales.results.length"
          @dataOptionsChange="fetchDataByOptions"
        )
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
      loading: false,
      sales,
      items: [],
      headers: [
        { text: 'Title', value: 'user.title', align: 'start', sortable: false, width: '50px' },
        { text: 'Full Name', value: 'userFullName', align: 'start', sortable: false, width: '20%' },
        { text: 'Email', value: 'email', align: 'start', sortable: false, width: '25%' },
        { text: 'Gender', value: 'gender', align: 'center', sortable: false },
        { text: 'Year', value: 'year', align: 'center', sortable: false },
        { text: 'Sales', value: 'sales', align: 'center', sortable: false },
        { text: 'Country', value: 'country', align: 'center', sortable: false, width: '20%' },
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
    this.items = await this.fetchData(0, 25)
  },
  methods: {
    async fetchDataByOptions(payload) {
      console.log(payload)
      this.loading = true
      this.items = await this.fetchData(payload.page -1, payload.itemsPerPage, payload.search)
      this.loading = false
    },
    async fetchData(page, size, search = '') {
      const start = page * size
      await this.delay(1000)
      let itemsToSlice = await sales.results
      if (search) {
        const lowerCasedSearch = search.toLowerCase()
        itemsToSlice = itemsToSlice.filter(
          (item) => item.email.toLowerCase().includes(lowerCasedSearch) || item.user.first_name.toLowerCase().includes(lowerCasedSearch)
            || item.user.last_name.toLowerCase().includes(lowerCasedSearch) || item.user.title.toLowerCase().includes(lowerCasedSearch)
            || item.gender.toLowerCase().includes(lowerCasedSearch) || String (item.year).includes(lowerCasedSearch)
            || item.sales.toLowerCase().includes(lowerCasedSearch) || item.country.toLowerCase().includes(lowerCasedSearch)
        )
      }
      return itemsToSlice.slice(start, start + size)
    },
    delay(ms) {
      return new Promise(resolve => setTimeout(resolve, ms))
    },
    filterBySearchStr(item) {

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
