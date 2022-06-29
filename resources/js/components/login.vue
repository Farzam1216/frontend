<template>
    <div class="container">
        <div class="row">
        <div class="col-sm-9 col-md-7 col-lg-5 mx-auto">
            <div class="card border-0 shadow rounded-3 my-5">
            <div class="card-body p-4 p-sm-5">
                <h5 class="card-title text-center mb-5 fw-light fs-5">Sign In</h5>

                <div class="form-floating mb-3">
                    <input type="email" v-model="email" class="form-control" id="floatingInput" placeholder="name@example.com">
                    <label for="floatingInput">Email address</label>
                </div>
                <div class="form-floating mb-3">
                    <input type="password" v-model="password" class="form-control" id="floatingPassword" placeholder="Password">
                    <label for="floatingPassword">Password</label>
                </div>

                <div class="form-check mb-3">
                    <input class="form-check-input" type="checkbox" value="" id="rememberPasswordCheck">
                    <label class="form-check-label" for="rememberPasswordCheck">
                    Remember password
                    </label>
                </div>
                <div class="form-check mb-3">
                    <a href="/register">Signup</a>
                </div>
                <div class="d-grid">
                    <p v-if="errors.length">
                            <b class="text-danger">Please correct the following errors:</b>
                                <ul >
                                    <li class="text-danger" v-for="error in errors" :key="error.id">{{ error }}</li>
                                </ul>
                            </p>
                    <button  v-if="status == false" @click="login()" class="btn btn-primary btn-lg btn-block">Login</button>
                    <a v-if="status == true" href="/category"  class="btn btn-primary btn-lg btn-block">Dashboard</a>
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
                password:'',
                errors: [],
                status:false,
                message:null,
            }
        },
         methods: {
           login(){
             this.errors = [];
            if (!this.email) {
                this.errors.push("Email Required.");
            }
            if (!this.password) {
                this.errors.push("Password Required.");
            }
             axios.post('http://192.168.18.27:8000/api/login',{
				email : this.email,
				password : this.password,
				}).then( (response) => {
                    this.message = response.data.data.message;
                    this.status = true;
                    swal({
                            title: "Login Successfull!",
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
