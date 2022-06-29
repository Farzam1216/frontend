<template>
    <div class="user-dashboard">
    <div class="row">
        <div class="col-sm-12">
            <!-- ADD Modal -->
            <!-- Small modal -->
                <button type="button" class="btn btn-primary" data-toggle="modal" data-target=".add-example-modal-lg">Create</button>

                <div class="modal fade add-example-modal-lg" tabindex="-1" role="dialog" aria-labelledby="mySmallModalLabel" aria-hidden="true">
                    <div class="modal-dialog modal-lg">
                        <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="exampleModalLabel">New category</h5>
                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                            </button>
                        </div>
                        <div class="modal-body">
                            <form>
                            <div class="form-group">
                                <label for="recipient-name" class="col-form-label">Name:</label>
                                <input type="text" v-model="category" class="form-control" id="recipient-name">
                            </div>
                            </form>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                            <button type="button" @click="add()" data-dismiss="modal"  class="btn btn-primary">Add</button>
                        </div>
                        </div>
                    </div>
                </div>
            <!-- Small modal -->
            <!-- end ADD Modal -->
            <table class="table">
                <tr>
                    <th>#</th>
                    <th>Category Name</th>
                    <th>Action</th>
                </tr>

                <tr v-for="data in data" :key="data.id">
                    <th>{{ data.id }}</th>
                    <th>{{data.name}}</th>
                    <th>
                        <button type="button" class="btn btn-primary text-dark" data-toggle="modal" :data-target="'#edit'+data.id">Edit</button>
                        <button type="button" @click="deletes(data.id)" class="btn btn-primary text-dark" >Delete</button>
                        <!-- edit -->
                        <div class="modal fade edit-example-modal-lg"  :id="'edit'+data.id" tabindex="-1" role="dialog" aria-labelledby="mySmallModalLabel" aria-hidden="true">
                    <div class="modal-dialog modal-lg">
                        <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="exampleModalLabel">New message</h5>
                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                            </button>
                        </div>
                        <div class="modal-body">
                            <form>
                            <div class="form-group">
                                <label for="recipient-name" class="col-form-label">Recipient:</label>
                                <input type="text" class="form-control" id="recipient-name">
                            </div>
                            </form>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                            <button type="button"  class="btn btn-primary">Send message</button>
                        </div>
                        </div>
                    </div>
                </div>
                <!-- edit -->
                    </th>
                </tr>
            </table>
        </div>
    </div>
</div>
</template>

<script>
import swal from 'sweetalert';
    export default {
        mounted() {
            console.log('Component mounted.')
            this.getCategory();
        }, data() {
            return {
                category:'',
                editcategory:'',
                errors: [],
                data:[],
            }
        },
         methods: {
            getCategory(){
                 const headers = {
                    'Authorization': 'Bearer '+my-token,
                    'Content-Type': 'application/json'
                 };
                axios
                    .get("http://192.168.18.27:8000/api/category")
                    .then((response) => {
                    this.data = response.data.data;
                        console.log(this.data);
                    })
                    .catch((error) => {
                    console.error(error);
                    });
            },
            add(){
                axios.post('http://192.168.18.27:8000/api/category',{
				name : this.category,
				}).then( (response) => {
                    swal({
                            title: "Category Added Successfull!",
                            text: "Category Added Successfull.",
                            icon: "success",
                            buttons: true,
                            timer: 3000
                        })
                        this.getCategory();
                });
            },
            update(){
                axios.put('http://192.168.18.27:8000/api/category',{
				name : this.category,
				}).then( (response) => {
                    swal({
                            title: "Category Updated Successfull!",
                            text: "Category Updated Successfull.",
                            icon: "success",
                            buttons: true,
                            timer: 3000
                        })
                        this.getCategory();
                });
            },
            deletes(category_id){
                axios.delete('http://192.168.18.27:8000/api/category/'+category_id)
             .then(response => {
                swal({
                            title: "Category Deleted Successfull!",
                            text: "Category Deleted Successfull.",
                            icon: "success",
                            buttons: true,
                            timer: 3000
                        })
                 this.getCategory();
             })
             .catch(function (error) {
                console.log(error.response)
             })
            }
         }
    }
</script>
