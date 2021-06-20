<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>LIST CUSTOMER</h4>
                        <hr>
                        <router-link :to="{name: 'post.create'}" class="btn btn-md btn-success">ADD DATA</router-link>

                        <table class="table table-striped table-bordered mt-4">
                            <thead class="thead-dark">
                                <tr>
                                    <th scope="col">Name</th>
                                    <th scope="col">Gender</th>
                                    <th scope="col">Options</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(post, index) in posts" :key="index">
                                    <td>{{ post.name }}</td>
                                    <td>{{ post.gender }}</td>
                                    <td class="text-center">
                                        <router-link :to="{name: 'post.edit', params:{id: post.id }}" class="btn btn-sm btn-info mr-1">Update</router-link>&nbsp;
                                        <router-link :to="{name: 'post.detail', params:{id: post.id }}" class="btn btn-sm btn-primary mr-1">Detail</router-link>&nbsp;
                                        <button type="button" @click.prevent="postDelete(index,post.id)" class="btn btn-sm btn-danger ml-1">DELETE</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import { onMounted, ref } from 'vue'

export default {

    setup() {

        let posts = ref([])

        onMounted(() => {

            //get API from Laravel Backend
            axios.get('http://localhost:8000/api/post')
            .then(response => {
              
              //assign state posts with response data
              posts.value = response.data.result;

            }).catch(error => {
                console.log(error.response.data)
            })

        })

        //method delete
        function postDelete(index,id) {
                    
        //delete data by ID
        axios.delete(`http://localhost:8000/api/post/${id}`)
        .then(() => {
                    
            //splice data list
            this.posts.splice(index, 1);

            }).catch(error => {
                console.log(error.response.data)
            })

        }

        //return
        return {
            posts,
            postDelete
        }

    }

}
</script>

<style>
    body{
        background: lightgray;
    }
</style>