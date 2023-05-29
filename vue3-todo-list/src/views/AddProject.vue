<template>
  <form @submit.prevent="handleSubmit">
    <label>Title</label>
    <input type="text" v-model="title" required>
    <label>Details</label>
    <textarea v-model="details" required></textarea>
    <button>Add DoMe</button>
  </form>
</template>

<script>
export default {
  data () {
    return {
      title: '',
      details: '',
      projects: []
    }
  },
  mounted () {
    fetch('http://localhost:3000/projects')
      .then(res => res.json())
      .then(data => {
        this.projects = data
        console.log('add-projects', this.projects)
      })
      .catch(err => console.log(err))
  },
  methods: {
    handleSubmit () {
      let project = {
        title: this.title,
        details: this.details,
        complete: false,
        id: Math.floor(Math.random() * 10000)
      }
      console.log('checkstatus', project, this.checkDuplicate(project))
      if (this.checkDuplicate(project)) {
        if (confirm('The list with this title already created!\nDo you still want to create a list with this title?') == true) {
          this.postProject(project)
        }
      } else {
        this.postProject(project)
      }

    },
    postProject (project) {
      fetch('http://localhost:3000/projects', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify(project)
      }).then(() => {
        this.$router.push('/')
      }).catch(err => console.log(err))
    },
    checkDuplicate (item) {
      let status = []
      if (this.projects && this.projects.length) {
        status = this.projects.filter((i) => {
          return i.title === item.title
        })
      }
      console.log('status', status)
      return status.length > 0
    }
  }
}
</script>

<style>
  form {
    background: #1d1d1d;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    padding: 20px;
    border-radius: 10px;
  }
  label {
    display: block;
    color: #bbb;
    text-transform: uppercase;
    font-size: 14px;
    font-weight: bold;
    letter-spacing: 1px;
    margin: 20px 0 10px 0
  }
  input {
    padding: 10px;
    border: 0;
    border-bottom: 1px solid #ddd;
    width: 100%;
    border-radius: 5px;
    box-sizing: border-box;
  }
  textarea {
    border: 1px solid #ddd;
    padding: 10px;
    border-radius: 5px;
    width: 100%;
    box-sizing: border-box;
    height: 100px;
  }
  form button {
    display: block;
    margin: 20px auto 0;
    background: #00ce89;
    color: white;
    padding: 10px;
    border: 0;
    border-radius: 6px;
    font-size: 16px;
  }
</style>