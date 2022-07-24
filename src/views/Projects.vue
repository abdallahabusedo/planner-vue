<template>
  <div class="home">
    <FilterNav @filterChange="current = $event" :current="current" />
    <div v-if="projects.length">
      <div v-for="project in filteredProject" :key="project.id">
        <SingleProject 
          :project="project" 
          @delete="handleDeleteProject" 
          @complete="handleCompleteProject" 
        />
      </div>
    </div>
  </div>
</template>
 
<script>
import SingleProject from "../components/SingleProject.vue";
import FilterNav from "../components/FilterNav.vue";

export default {
    name: "projects",
    data() {
        return {
            projects: [],
            current: 'all',
        };
    },
    mounted() {
        fetch("http://localhost:3000/projects")
            .then(res => res.json())
            .then(data => this.projects = data)
            .catch(e => console.log(e.message));
    },
    methods: {
      handleDeleteProject(id) {  
        this.projects = this.projects.filter((project) => project.id !== id )      
        },
      handleCompleteProject(id){
        let p = this.projects.find((project) =>  project.id === id )
        p.complete = !p.complete
      }
    },
    components: { SingleProject, FilterNav },
    computed: { 
      filteredProject(){
        if (this.current === 'completed'){
          return this.projects.filter(project=> project.complete )
        }
        if (this.current === 'ongoing'){
          return this.projects.filter(project=> !project.complete )
        }
        return this.projects
      } 
    }
}
</script>

<style>

</style>