<template>
    <h1>
        <i class="fa-solid fa-plus"></i>
        Nueva categoria
    </h1>
    <form>
        <div class="mb-3">
            <label for="name" class="form-label">Nombre</label>
            <input type="text" class="form-control" id="name" v-model="category.name">
        </div>
        <div class="mb-3">
            <label for="description" class="form-label">Descripción</label>
            <input type="text" class="form-control" id="description" v-model="category.description">
        </div>
        <button class="btn btn-outline-success" @click="newCategory">Guardar</button> 
    </form>
</template>
<script>
    export default {
        data(){
            return{
                category: {
                    name: "",
                    description: ""
                }
            }
        },
        methods:{
            async newCategory(e){
                e.preventDefault();
                const options = {
                method: "POST",
                headers: { 
                    'Content-Type': 'application/json'
                },
                body:  JSON.stringify(this.category)
            }
                const response = await fetch("http://127.0.0.1:8000/api/category/store", options);
                const data = await response.json();
                //console.log(data);
                this.$router.replace({path: '/categories'});
            }
        }
        
    }
</script>
<style scoped>
    form{
        width: 400px;
        margin: 2em auto;
    }
    h1{
        font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
        text-align: center;
    }
</style>