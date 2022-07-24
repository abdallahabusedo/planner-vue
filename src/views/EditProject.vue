<template>
  <h1>Edit Project</h1>
  <form @submit.prevent="handleSubmit" >
      <label for="">Title: </label>
      <input type="text" v-model="title" required>
      <label for="">Details</label>
      <textarea required v-model="details"></textarea>
      <button>Edit Project</button>
    </form>
</template>

<script>
export default {
    props: ['id'],
    data() {
        return{
            title:'',
            details:'',
            uri: "http://localhost:3000/projects/" + this.id,
        }
    },
    mounted(){
        fetch(this.uri)
        .then(res => res.json())
        .then(data => {
            console.log(data);
            this.title = data.title
            this.details = data.details
        })
    },
     methods: {
    handleSubmit() {
      fetch(this.uri, {
        method: 'PATCH',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            title: this.title, 
            details: this.details ,
        }),
        }).then(()=>{
          this.$router.push('/');
        }).catch(e => console.log(e.message))
    }
  }

}
</script>

<style>

</style>