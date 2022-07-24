<template>
    <div class="project" :class="{complete : project.complete}">
        <div class="actions" >
            <h3 @click="ToggleDetails"> {{project.title}}</h3>
            <div class="icons">
                <router-link :to="{name: 'EditProject' , params: {id : project.id}}" >
                    <span class="material-icons"  >edit</span>
                </router-link>
                <span class="material-icons" @click="DeleteProject">delete</span>
                <span class="material-icons tick" @click="ToggleProgress">done</span>
            </div>
        </div>
        <div v-if="showDetails" class="details">
            <p>{{project.details}}</p>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            showDetails: false,
            uri: "http://localhost:3000/projects/"+this.project.id,
        }
    },
    methods: {
        ToggleDetails() {
            this.showDetails = !this.showDetails
        },
        DeleteProject(){
            fetch(this.uri,{method: 'DELETE'})
            .then(()=>this.$emit('delete',this.project.id))
            .catch(err=>console.log(err))
        },
        ToggleProgress(){
            fetch(this.uri,{
                method: 'PATCH',
                headers: {'Content-Type': 'application/json'},
                body: JSON.stringify({complete: !this.project.complete}),
                }).then(()=> {this.$emit('complete',this.project.id)})
                .catch(err=>console.log(err))
        }
    },
    
    props: ['project'],
}
</script>

<style>
    .project {
        margin: 20px auto;
        background: white;
        padding: 10px 20px;
        border-radius: 4px;
        box-shadow: 1px 2px 3px rgba(0, 0, 0, 0.5);
        border-left: 5px solid red;
    }
    h3 {
        cursor: pointer;
    }
    .actions{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .material-icons{
        font-size: 24px;
        margin-left: 10px;
        color: #bbb;
        cursor: pointer;
    }
    .material-icons:hover{
        color: #777;
    }
    .project.complete{
        border-left:5px solid green;
    }
    .project.complete .tick{
        color: green;
    }
</style>