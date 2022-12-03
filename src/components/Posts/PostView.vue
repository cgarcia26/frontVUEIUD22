<template>
    <h1>
        <i class="fa fa-tag" aria-hidden="true"></i>
        Lista de Post
        <router-link :to="'create'">
            <button class="btn btn-outline-success" >
                <i className="fa-solid fa-plus"></i>    
            </button>
        </router-link>
    </h1>

    <table class="table table-striped table-hover">
        <thead>
            <th>#</th>
            <th>Nombre</th>
            <th>Descripción</th>
            <th>Contenido</th>
            <th>Categoria</th>
            <th>Usuario</th>
            <th>Fecha creación</th>
            <th>Fecha actualización</th>
            <th>Acciones</th>
        </thead>
    <tbody>
            <tr v-for="post in posts" :key="post.id">
                <td>{{ post.id }}</td>
                <td>{{ post.name }}</td>
                <td>{{ post.description }}</td>
                <td>{{ post.content }}</td>
                <td>{{ post.categoryName }}</td>
                <td>{{ post.userName }}</td>
                <td>{{ dateFormat(post.created_at) }}</td>
                <td>{{ dateFormat(post.updated_at) }}</td>
                <td>
                    <router-link :to=" post.id + '/edit'">
                        <button class="btn btn-outline-primary">
                            <i className="fa-solid fa-pen-to-square"></i>
                        </button>
                    </router-link>
                    <button class="btn btn-outline-danger" href="javascript:void(0)" @click="deletePost(post.id)">
                        <i className="fa-solid fa-trash"></i>
                    </button>
                </td>
            </tr>
        </tbody>
    </table>
</template>

<script>
    export default {
       async created(){
            const options = {
                method: "GET"
            }
            const response = await fetch("http://localhost:8000/api/post", options);
            const data = await response.json();
            //console.log(data);
            this.posts = data.data;
        },
        data(){
            return{
                posts: []
            }
        },
       methods:{
            async deletePost(id){
                //console.log(id);
                const options = {
                method: "DELETE",
                headers: { 
                    'Content-Type': 'application/json'
                },
            }
                const response = await fetch("http://localhost:8000/api/post/"+ id +"/destroy", options);
                const data = await response.json();
                // console.log(data.data.newList);
                this.postsAll();           
            },
            async postsAll(){
            const options = {
                method: "GET"
            }
            const response = await fetch("http://localhost:8000/api/post", options);
            const data = await response.json();
            //console.log(data);
            this.posts = data.data;
            },
            dateFormat(date){
                const dateFormat = new Date(date)
                return dateFormat.toLocaleDateString('es-co', { year:"numeric", month:"short", day:"numeric"})  
            },
        },
    }
</script>

<style scoped>
    button{
        margin: 1%
    }
    h1{
        font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
        text-align: center;
        text-shadow: 1px 1px 1px black, 2px 2px 1px blue;  
    }
</style>
