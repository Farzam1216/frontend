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
                            <h5 class="modal-title" id="exampleModalLabel">New Car</h5>
                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                            <span aria-hidden="true">&times;</span>
                            </button>
                        </div>
                        <div class="modal-body">
                            <form>
                            <div class="form-group">
                                <label for="recipient-name" class="col-form-label">Select Category:</label>
                                <select name="" v-model="category" id="" class="form-control">
                                    <option v-for="data in data" :key="data.id" :value="data.id">{{ data.name }}</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="recipient-name" class="col-form-label">Name:</label>
                                <input type="text" v-model="name" class="form-control" id="recipient-name">
                            </div>
                            <div class="form-group">
                                <label for="recipient-name" class="col-form-label">Color:</label>
                                <input type="text" v-model="color" class="form-control" id="recipient-name">
                            </div>
                            <div class="form-group">
                                <label for="recipient-name" class="col-form-label">Model:</label>
                                <input type="text" v-model="model" class="form-control" id="recipient-name">
                            </div>
                            <div class="form-group">
                                <label for="recipient-name" class="col-form-label">Registration:</label>
                                <input type="text" v-model="registration" class="form-control" id="recipient-name">
                            </div>
                             <div class="form-group">
                                <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                            <button type="button" @click="add()" data-dismiss="modal"  class="btn btn-primary">Add</button>

                            </div>
                            <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
                            <button type="button" @click="add()" data-dismiss="modal"  class="btn btn-primary">Add</button>
                        </div>
                            </form>
                        </div>

                        </div>
                    </div>
                </div>
            <!-- Small modal -->
            <!-- end ADD Modal -->
            <table class="table">
                <tr>
                    <th>Name</th>
                    <th>Color</th>
                    <th>Model</th>
                    <th>Registration</th>
                    <th>Action</th>
                </tr>

                <tr v-for="cars in cars" :key="cars.id">
                    <th>{{cars.name}}</th>
                     <th>{{cars.color}}</th>
                      <th>{{cars.model}}</th>
                       <th>{{cars.registration}}</th>
                    <th>
                        <button type="button" class="btn btn-primary text-dark" data-toggle="modal" :data-target="'#edit'+cars.id">Edit</button>
                        <button type="button" @click="deletes(cars.id)" class="btn btn-primary text-dark" >Delete</button>
                        <!-- edit -->
                        <div class="modal fade edit-example-modal-lg"  :id="'edit'+cars.id" tabindex="-1" role="dialog" aria-labelledby="mySmallModalLabel" aria-hidden="true">
                    <div class="modal-dialog modal-lg">
                        <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="exampleModalLabel">New car</h5>
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
                            <button type="button"  class="btn btn-primary">update</button>
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
            this.getCars();
        }, data() {
            return {
                category:'',
                name:'',
                model:'',
                color:'',
                registration:'',
                errors: [],
                data:[],
                cars:[],
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
            getCars(){
                const headers = {
                    'Authorization': 'Bearer '+my-token,
                    'Content-Type': 'application/json'
                 };
                axios
                    .get("http://192.168.18.27:8000/api/car")
                    .then((response) => {
                    this.cars = response.data.data;
                        console.log(this.cars);
                    })
                    .catch((error) => {
                    console.error(error);
                    });
            },
            add(){
                const headers = {
                    'Authorization': 'Bearer '+my-token,
                    'Content-Type': 'application/json'
                 };
                axios.post('http://192.168.18.27:8000/api/car',{
				category_id : this.category,
                name : this.name,
                model : this.model,
                color : this.color,
                registration : this.registration,
				}).then( (response) => {
                    swal({
                            title: "Car Added Successfull!",
                            text: "Car Added Successfull.",
                            icon: "success",
                            buttons: true,
                            timer: 3000
                        })
                        this.getCategory();
                         this.getCars();
                });
            },
            update(){
                const headers = {
                    'Authorization': 'Bearer '+my-token,
                    'Content-Type': 'application/json'
                 };
                axios.put('http://192.168.18.27:8000/api/category',{
				category_id : this.category,
                name : this.name,
                model : this.model,
                color : this.color,
                registration : this.registration,
				}).then( (response) => {
                    swal({
                            title: "Car Updated Successfull!",
                            text: "Car Updated Successfull.",
                            icon: "success",
                            buttons: true,
                            timer: 3000
                        })
                        this.getCategory();
                        this.getCars();
                });
            },
            deletes(car_id){
                const headers = {
                    'Authorization': 'Bearer '+my-token,
                    'Content-Type': 'application/json'
                 };
                axios.delete('http://192.168.18.27:8000/api/car/'+car_id)
             .then(response => {
                swal({
                            title: "Car Deleted Successfull!",
                            text: "Car Deleted Successfull.",
                            icon: "success",
                            buttons: true,
                            timer: 3000
                        })
                 this.getCategory();
                  this.getCars();
             })
             .catch(function (error) {
                console.log(error.response)
             })
            }
         }
    }
</script>
