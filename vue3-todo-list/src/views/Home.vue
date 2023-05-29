<template>
  <div class="home">
    <FilterNav :current="current" @filterChange="current = $event" @filterName="filterName" />
    <div v-if="filterResult.length">
      <div v-for="project in filterResult" :key="project.id">
      <SingleProject :project="project" @delete="handleDelete" @complete="handleComplete" />
      </div>
    </div>
    <div v-else>
      <h2
        style="font-style: italic; text-align: center;"
      >
        No list found!<br>Create a new one instead!
    </h2>
    </div>
  </div>
</template>

<script>
// challenge
//   - when the filter changes, only show those projects
//   - e.g. if we click 'completed' only show completed project
//   - use a computed property to do this

import SingleProject from '../components/SingleProject.vue'
import FilterNav from '../components/FilterNav.vue'

export default {
  name: 'Home',
  components: { SingleProject, FilterNav },
  data() {
    return {
      projects: [],
      current: 'all',
      search: ''
    };
  },
  mounted() {
    fetch('http://localhost:3000/projects')
      .then(res => res.json())
      .then(data => this.projects = data)
      .catch(err => console.log(err))
  },
  methods: {
    handleDelete(id) {
      this.projects = this.projects.filter(project => {
        return project.id !== id
      })
    },
    handleComplete(id) {
      let p = this.projects.find(project => {
        return project.id === id
      })
      p.complete = !p.complete 
    },
    filterName (val) {
      this.search = val
      console.log('search', val)
    }
  },
  computed: {
    filteredProjects () {
      if (this.current === 'completed') {
        return this.projects.filter(project => project.complete)
      }
      if (this.current === 'ongoing') {
        return this.projects.filter(project => !project.complete)
      }

      return this.projects
    },
    filterResult () {
      let result = []
      if (this.search) {
        result = this.filteredProjects.filter((p) => {
          return p.title.toLowerCase().includes(this.search.toLowerCase().trim())
        })
      } else {
        result = this.filteredProjects
      }
      return result
    },
  },
}
</script>
