<template>
  <div class="home">
   <h1>Home</h1>
   <FilterNav @filterValue="current=$event" :current="current"></FilterNav>
   <div v-for="project in filterProjects" :key="project.id">
   <SingleProject :project="project" @delete="deleteProject" @complete="completeProject">

   </SingleProject>
  </div>
   
  </div>
  {{ current }}
 
</template>

<script>



import FilterNav from '../components/FilterNav'
import SingleProject from '../components/SingleProject'
export default {
  name: 'HomeView',
  
  components: {
    FilterNav,
    SingleProject,
    
  },
  data() {
    return {
      projects: [],
      current:"all"
      
    }
  },
  methods: {
    deleteProject(id) {
      this.projects = this.projects.filter((project) => {
        return id !== project.id;
    })
    },
    completeProject(id) {
     
      let project = this.projects.find((project) => {
        return project.id === id;
      })
      project.complete = !project.complete;
   }
  },
  computed: {
    filterProjects() {
      if (this.current === "complete") {
        return this.projects.filter((project) => {
          return project.complete;
        })
      }
      if (this.current === "ongoing") {
        return this.projects.filter((project) => {
          return project.complete === false;
        })
      }
      return this.projects;
    }
  },
  mounted() {
    fetch("http://localhost:3000/projects").
      then((response) => {
        return response.json();
      })
      .then((datas) => {
        this.projects = datas;
       
      })
      .catch((err) => {
      console.log(err)
    })
  }
}
</script>
