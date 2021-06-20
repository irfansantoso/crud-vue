<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>UPDATE CUSTOMER</h4>
                        <hr>

                        <form @submit.prevent="update">
                            <div class="form-group">
                                <label for="name" class="font-weight-bold">Name</label>
                                <input type="text" class="form-control" v-model="post.name" placeholder="Masukkan Name">
                                <!-- validation -->
                                <div v-if="validation.name" class="mt-2 alert alert-danger">
                                    {{ validation.name[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="email" class="font-weight-bold">Email</label>
                                <input type="text" class="form-control" v-model="post.email" placeholder="Masukkan Email">
                                <!-- validation -->
                                <div v-if="validation.email" class="mt-2 alert alert-danger">
                                    {{ validation.email[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="gender" class="font-weight-bold">Gender</label>
                                <select v-model="post.gender" class="form-control">
                                    <option value="man">Man</option>
                                    <option value="women">Women</option>
                                </select>                                
                            </div>
                            <div class="form-group">
                                <label for="password" class="font-weight-bold">Password</label>
                                <input type="text" class="form-control" v-model="post.password" placeholder="Masukkan Password">
                                <!-- validation -->
                                <div v-if="validation.password" class="mt-2 alert alert-danger">
                                    {{ validation.password[0] }}
                                </div>
                            </div>
                            <div class="form-group">
                                <label for="is_married" class="font-weight-bold">Married</label>
                                <br>
                                <input class="form-check-input" type="radio" name="is_married" id="yes" value="yes" v-model="post.is_married">
                                <label for="yes">Yes</label>
                                <input class="form-check-input" type="radio" name="is_married" id="no" value="no" v-model="post.is_married">
                                <label for="no">No</label>
                                <br>
                            </div>
                            <div class="form-group">
                                <label for="address" class="font-weight-bold">address</label>
                                <textarea class="form-control" rows="4" v-model="post.address" placeholder="Masukkan Addresst"></textarea>
                                <!-- validation -->
                                <div v-if="validation.address" class="mt-2 alert alert-danger">
                                    {{ validation.address[0] }}
                                </div>
                            </div>
                            <button type="submit" class="btn btn-primary">UPDATE</button>
                        </form>                        

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { reactive, ref, onMounted } from 'vue'
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

        //state validation
        const validation = ref([])

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

        //method update
        function update() {

            let name   = post.name
            let email = post.email
            let password = post.password
            let gender = post.gender
            let is_married = post.is_married
            let address = post.address

            axios.put(`http://localhost:8000/api/post/${route.params.id}`, {
                name: name,
                email: email,
                password: password,
                gender: gender,
                is_married: is_married,
                address: address

            }).then(() => {

                router.push({
                    name: 'post.index'
                })

            }).catch(error => {
                validation.value = error.response.data
            })

        }

        //return
        return {
            post,
            validation,
            router,
            update
        }

    }

}
</script>

<style>
    body{
        background: lightgray;
    }
</style>