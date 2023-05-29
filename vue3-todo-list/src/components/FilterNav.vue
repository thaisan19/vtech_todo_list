<template>
  <nav class="filter-nav">
    <button 
      @click="updateFilter('all')" 
      :class="{ active: current === 'all'}"
    >View all</button>
    <button 
      @click="updateFilter('completed')"
      :class="{ active: current === 'completed'}"
    >Completed</button>
    <button 
      @click="updateFilter('ongoing')"
      :class="{ active: current === 'ongoing'}"
    >Ongoing</button>
  </nav>
  <div class="name_filter">
    <input
      type="text"
      v-model="search"
      placeholder="Search by title:"
    >
  </div>
</template>

<script>
export default {
  props: ['current'],
  data () {
    return {
      search: ''
    }
  },
  watch: {
    search (cur) {
      this.$emit('filterName', cur)
    }
  },
  methods: {
    updateFilter(by) {
      this.$emit('filterChange', by)
    }
  }
}
</script>

<style>
  .filter-nav button {
    background: none;
    border: none;
    color: #bbb;
    outline: none;
    font-size: 12px;
    text-transform: uppercase;
    margin-right: 10px;
    letter-spacing: 1px;
    font-weight: bold;
    cursor: pointer;
  }
  .filter-nav button.active {
    color: #555;
  }
  input {
    margin-top: 16px;
    padding: 10px;
    border: 0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    border-radius: 5px;
    box-sizing: border-box;
  }
</style>