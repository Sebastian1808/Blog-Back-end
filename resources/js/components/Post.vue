<template>
    <div>
        <h1 class="text-center"> Administracion de post's</h1>
        <hr>
                <!-- Button trigger modal -->
            <button @click="update=false; openModal();" type="button" class="btn btn-primary mb-3">
                Nuevo Post
            </button><br>

            <!-- Modal -->
            <div class="modal" :class="{show:modal}">
            <div class="modal-dialog">
                <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">{{titleModal}}</h5>
                    <button @click="closeModal();" type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <div>
                        <label for="name">Nombre del autor</label>
                        <input v-model="post.name" type="text" class="form-control" id="name" placeholder="Nombre del autor" name="">
                    </div>
                    <div>
                        <label for="title">Titulo del articulo</label>
                        <input v-model="post.title" type="text" class="form-control" id="title" placeholder="Titulo del articulo" name="">
                    </div>
                    <div>
                        <label for="content">Contenido del articulo</label>
                        <input v-model="post.content" type="text" class="form-control" id="content" placeholder="Contenido del articulo" name="">
                    </div>
                </div>
                <div class="modal-footer">
                    <button @click="closeModal();" type="button" class="btn btn-secondary" data-dismiss="modal">Cerrar</button>
                    <button @click="guardar();" type="button" class="btn btn-success">Guardar cambios</button>
                </div>
                </div>
            </div>
            </div>
        <table class="table table-striped table-hover">
            <thead class=" thead-dark">
                <tr>
                <th scope="col">Id</th>
                <th scope="col">Autor</th>
                <th scope="col">Titulo</th>
                <th scope="col">Contenido</th>
                <th scope="col" colspan="2" class="text-center">Accion</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="post in posts" :key="post.id">
                    <th scope="row">{{ post.id }}</th>
                        <td>{{ post.name }}</td>
                        <td>{{ post.title }}</td>
                        <td>{{ post.content }}</td>
                        <td>
                            <button @click="update=true; openModal(post);" class="btn btn-warning">Editar</button>
                        </td>
                        <td>
                            <button @click="eliminar(post.id)" class="btn btn-danger">Eliminar</button>
                        </td>
                </tr>
                
            </tbody>
            </table>
    </div>
</template>

<script>
    export default {
        data(){
            return {
                post:{
                    name:'',
                    title:'',
                    content:''
                },
                id:0,
                update:true,
                modal:0,
                titleModal: '',
                posts:[],
            }
        },
        methods: {
            async list(){
                const res = await axios.get('posts');
                this.posts = res.data;
            },
            async eliminar(id){
                const res = await axios.delete('/posts/' +id);
                this.posts = res.data;
                this.list();
            },
            async guardar(){
                if(this.update){
                    const res = await axios.put('/posts/'+ this.id, this.post);

                }else{
                    const res = await axios.post('/posts', this.post);
                }
                this.closeModal();
                this.list();
            },
             openModal(data={}){
                this.modal=1;
                if (this.update){
                    this.id = data.id;
                    this.titleModal="Modificar Post";
                    this.post.name= data.name;
                    this.post.title= data.title;
                    this.post.content= data.content;
                }else{
                    this.id = 0;
                    this.titleModal="Crear Post";
                    this.post.name="";
                    this.post.title='';
                    this.post.content='';
                }
            },
            closeModal(){
                this.modal=0;
            },
        },
        created() {
            this.list();
        },
    
    }
</script>

<style>
    .show {
        display: list-item;
        opacity: 1;
        background: rgba(44, 38, 75, 0.849);
    }

</style>