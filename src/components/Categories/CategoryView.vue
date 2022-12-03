<template>
    <h1>
        <i class="fa fa-book" aria-hidden="true"></i>
        Lista de Categorias
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
            <th>Fecha creación</th>
            <th>Fecha actualización</th>
            <th>Acciones</th>
        </thead>
        <tbody>
            <tr v-for="categories in categories" :key="categories.id">
                <td>{{ categories.id }}</td>
                <td>{{ categories.name }}</td>
                <td>{{ categories.description }}</td>
                <td>{{ dateFormat(categories.created_at) }}</td>
                <td>{{ dateFormat(categories.updated_at) }}</td>
                <td>
                    <router-link :to=" categories.id + '/edit'">
                        <button class="btn btn-outline-primary">
                            <i className="fa-solid fa-pen-to-square"></i>
                        </button>
                    </router-link>
                    <button class="btn btn-outline-danger" href="javascript:void(0)" @click="deleteCategory(categories.id)">
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
            const response = await fetch("http://localhost:8000/api/category", options);
            const data = await response.json();
            //console.log(data);
            this.categories = data.data;
        },
        data(){
            return{
                categories: []
            }
        },
       methods:{
            async deleteCategory(id){
                //console.log(id);
                const options = {
                method: "DELETE",
                headers: { 
                    'Content-Type': 'application/json'
                },
            }
                const response = await fetch("http://localhost:8000/api/category/"+ id +"/destroy", options);
                const data = await response.json();
                // console.log(data.data.newList);
                this.categoriesAll();           
            },
            async categoriesAll(){
            const options = {
                method: "GET"
            }
            const response = await fetch("http://localhost:8000/api/category", options);
            const data = await response.json();
            //console.log(data);
            this.categories = data.data;
            },
            dateFormat(created){
                const dateC = new Date(created)
                return dateC.toLocaleDateString('es-co', { year:"numeric", month:"short", day:"numeric"})  
            }
        }
    }
</script>

<style scoped>
    button{
        margin: 1%
    }
    h1{
        font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
        text-align: center;
        text-shadow: 1px 1px 1px black, 2px 2px 1px green;  
    }
</style>