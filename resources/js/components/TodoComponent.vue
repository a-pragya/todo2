<template>
    <div>
        <form @submit.prevent="saveData" class="needs-validation" >
            <div class="input-group mb-3" >
                <input v-model="newTodo.title" type="text" class="form-control form-control-lg" placeholder="Enter todo" required >
                <button class="btn btn-success" type="submit" id="button-addon2">Add</button>
            </div>
        </form>
        <div v-for="todo in todos" :key="todo.id" >
            <div class="card mt-2">
                <div class="card-body">
                    <h5 class="card-title text-center">{{todo.title}}</h5>
                        <div class="text-center">
                            <button @click="deleteTodo(todo.id)" class="btn btn-outline-danger btn-small">Delete</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    
</template>


<script>
    export default {
        data(){
            return{
                todos:'',
                newTodo:{}
                
            }
        },
        methods:{
            deleteTodo(e){
                
                console.log("id",e)
                axios.delete(`/api/todo/${e}`).then((res) =>{
                    
                    this.todos = res.data
                    
                }).catch((error)=>{
                    console.log("error",error)
                })
            },
            getTodos(){
                    axios.get('/api/todo').then((res) =>{
                        this.todos = res.data
                    }).catch((error) =>{
                        console.log(error)
                    })
            },
            saveData(){
                console.log("new todo",this.newTodo)
                axios.post('/api/todo', this.newTodo).then((res) =>{
                    this.newTodo.title=""
                     this.getTodos()
                }).catch((error) => {
                    console.log(error)
                })
            }
        },
        mounted() {
            this.getTodos()
        }
    }
</script>
