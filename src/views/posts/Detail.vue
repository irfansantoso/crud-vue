<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>DETAIL CUSTOMER</h4>
                        <hr>

                        <form>
                            <div class="form-group">
                                <label for="name" class="font-weight-bold">Name</label>
                                <p class="bg-light">{{post.name}}</p>
                                
                            </div>
                            <div class="form-group">
                                <label for="email" class="font-weight-bold">Email</label>
                                <p class="bg-light">{{post.email}}</p>
                            </div>
                            <div class="form-group">
                                <label for="gender" class="font-weight-bold">Gender</label>
                                <p class="bg-light">{{post.gender}}</p>                                
                            </div>
                            <div class="form-group">
                                <label for="password" class="font-weight-bold">Password</label>
                                <p class="bg-light">{{post.password}}</p>
                            </div>
                            <div class="form-group">
                                <label for="is_married" class="font-weight-bold">Married</label>
                                <br>
                                <p class="bg-light">{{post.is_married}}</p>
                            </div>
                            <div class="form-group">
                                <label for="address" class="font-weight-bold">Address</label>
                                <p class="bg-light">{{post.address}}</p>
                            </div>
                            <router-link :to="{name: 'post.index'}" class="btn btn-sm btn-primary mr-1">BACK</router-link>
                        </form>                        

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import axios from 'axios'

export default {

    setup() {

        const post = reactive({
            name: '',
            email: '',
            password: '',
            gender: '',
            is_married: '',
            address: ''
        })

        //vue router redirect
        const router = useRouter()

        //vue router get param 
        const route = useRoute()

        onMounted(() => {

            //get API from Laravel Backend
            axios.get(`http://localhost:8000/api/post/${route.params.id}`)
            .then(response => {
              
              //assign state posts with response data
              post.name    = response.data.result.name  
              post.email  = response.data.result.email
              post.password  = response.data.result.password
              post.gender  = response.data.result.gender
              post.is_married  = response.data.result.is_married
              post.address  = response.data.result.address

            }).catch(error => {
                console.log(error.response.data)
            })

        })

        

        //return
        return {
            post,
            router
            
        }

    }

}
</script>

<style>
    body{
        background: lightgray;
    }
</style>