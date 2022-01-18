<template lang="pug">
  v-card
    v-card-title
      v-select.column-filter(v-model='selectedHeaders', :items='headers', label='Show Columns', multiple='', item-text='text', item-value='value')
      v-spacer
      v-text-field.search-field(v-model='search', append-icon='mdi-magnify', label='Search Data', single-line='', hide-details='')
    v-data-table.elevation-1.mt-5(
      :loading='loading'
      :options.sync='options'
      :server-items-length='totalItemsCount'
      :headers='filteredHeaders'
      :items='items'
      item-key='email'
      :footer-props='{\
        itemsPerPageOptions: [25, 50, 100, 200]\
      }',
      dense='dense')
</template>

<script>
export default {
  props: ['headers', 'items', 'totalItemsCount', 'loading'],
  data() {
    return {
      selectedHeaders: [],
      search: '',
      options: {}
    }
  },
  watch: {
    options: {
      handler (data, oldData) {
        if (oldData.page) {
          const { itemsPerPage, page } = data
          this.$emit('dataOptionsChange', { page, itemsPerPage, search: this.search })
        }
      },
      deep: true,
    },
    search(searchStr) {
      this.$emit('dataOptionsChange', { page: this.options.page, itemsPerPage: this.options.itemsPerPage, search: searchStr })
    }
  },
  computed: {
    filteredHeaders() {
      return this.headers.filter((item => this.selectedHeaders.includes(item.value)))
    }
  },
  created() {
    this.selectedHeaders = this.headers.map((header) => header.value);
  },
}
</script>

<style lang="sass" scoped>
  .v-data-table
    max-width: 100%
  .column-filter
    width: 300px
  .search-field
    margin-top: -18px
  table, th, td
    border: 1px solid #ccc
    border-collapse: collapse
</style>
