<template>
    <div class="container">
        <div class="row">
        <div class="col-sm-9 col-md-7 col-lg-5 mx-auto">
            <div class="card border-0 shadow rounded-3 my-5">
            <div class="card-body p-4 p-sm-5">
                <h5 class="card-title text-center mb-5 fw-light fs-5">Sign Up</h5>
                <div class="form-floating mb-3">
                    <input type="text" v-model="name" class="form-control" id="floatingPassword" placeholder="Password">
                    <label for="floatingPassword">Name</label>
                </div>
                <div class="form-floating mb-3">
                    <input type="email" v-model="email" class="form-control" id="floatingInput" placeholder="name@example.com">
                    <label for="floatingInput">Email address</label>
                </div>
                <div class="form-check mb-3">
                    <a href="/">Login</a>
                </div>
                <div class="d-grid">
                    <p v-if="errors.length">
                            <b class="text-danger">Please correct the following errors:</b>
                                <ul >
                                    <li class="text-danger" v-for="error in errors" :key="error.id">{{ error }}</li>
                                </ul>
                            </p>
                    <button  v-if="status == false" @click="register()" class="btn btn-primary btn-lg btn-block">Register</button>
                    <button v-if="status == true" href='/' class="btn btn-primary btn-lg btn-block">login</button>
                </div>
            </div>
            </div>
        </div>
        </div>
    </div>
</template>

<script>
import swal from 'sweetalert';
    export default {
        mounted() {
        },
        data() {
            return {
                email:'',
                name:'',
                errors: [],
                status:false,
                message:null,
            }
        },
         methods: {
           register(){
             this.errors = [];
            if (!this.email) {
                this.errors.push("Email Required.");
            }
            if (!this.name) {
                this.errors.push("Name Required.");
            }
             axios.post('http://192.168.18.27:8000/api/register',{
				email : this.email,
				name : this.name,
				}).then( (response) => {
                    this.message = response.data.data.message;
                    this.status = true;
                    swal({
                            title: "Registered Successfull!",
                            text: response.data.data.message,
                            icon: "success",
                            buttons: true,
                            timer: 3000
                        })
                });
           }
        }
    }

</script>
