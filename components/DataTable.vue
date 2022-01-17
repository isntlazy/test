<template lang="html">
  <v-card>
    <v-card-title>
      <v-select
        class="column-filter"
        v-model="selectedHeaders"
        :items="headers"
        label="Show Columns"
        multiple
        item-text="text"
        item-value="value"
      >
      </v-select>
      <v-spacer></v-spacer>
      <v-text-field
        class="search-field"
        v-model="search"
        append-icon="mdi-magnify"
        label="Search Data"
        single-line
        hide-details
      ></v-text-field>
    </v-card-title>
    <transition name="fade">
      <v-data-table
        class="elevation-1 mt-5"
        :search="search"
        :headers="filteredHeaders"
        :items="items"
        item-key="email"
        dense="dense">
      </v-data-table>
    </transition>
  </v-card>
</template>

<script>
export default {
  props: ['headers', 'items'],
  data() {
    return {
      selectedHeaders: [],
      search: '',
      showTable: true
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
</style>
