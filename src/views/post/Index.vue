<template>
    <div class="container mt-5 overflow-auto">
        <div class="row">
            <div class="col-12">
                <div class="card border-0 rounded shadow-lg">
                    <div class="card-body">
                        <h4>DATA BARANG</h4>
                        <hr>
                        <router-link :to="{name: 'post.create'}" class="btn btn-md btn-success">TAMBAH BARANG</router-link>

                        <table class="table table-responsive table-striped table-bordered mt-4 border-1">
                            <thead class="thead bg-dark text-light">
                                
                                <tr>
                                    <div class="row">
                                    <div class="col-4"><th scope="col">NAMA</th></div>
                                    <div class="col-4"><th scope="col">HARGA</th></div>
                                    <div class="col-2 "><th scope="col">STOK</th></div>
                                    <div class="col-2 "><th scope="col">OPTIONS</th></div>
                                    </div>
                                </tr>
                                
                            </thead>
                            <tbody>
                                <tr v-for="(barang, index) in barangs" :key="index">
                                    <div class="row">
                                        <div class="col-4"><td>{{ barang.nama_barang }}</td></div>
                                        <div class="col-4"><td>{{ barang.harga }}</td></div>
                                        <div class="col-2"><td>{{ barang.stok }}</td></div>
                                        <div class="col-2"><td class="text-center">
                                        <router-link :to="{name: 'post.edit', params:{id: barang.id }}" class="btn btn-sm btn-primary mx-2">EDIT</router-link>
                                        <button @click.prevent="postDelete(barang.id)" class="btn btn-sm btn-danger ml-1">DELETE</button>
                                    </td></div>
                                    </div>
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
import {onMounted,ref} from 'vue'
export default {

    setup() {

        //reactive state
        let barangs = ref([])

        //mounted
        onMounted(() => {

            //get API from Laravel Backend
            axios.get('http://127.0.0.1:8000/api/barang')
            .then(response => {
              console.log(response.data)
              //assign state posts with response data
              barangs.value = response.data.barang

            }).catch(error => {
                console.log(error.response.data)
            })

        })

        function postDelete(id) {
            
   //delete data post by ID
   if (confirm("Anda Yakin?")) {
   axios.delete(`http://127.0.0.1:8000/api/barang/${id}`)
   .then(() => {
             alert("Data Dihapus!") 
       //splice posts 
       const index = this.barangs.findIndex(barang => barang.id === id) // find the post index
if (~index) {
// if the post exists in array
this.barangs.splice(index, 1)
}

    }).catch(error => {
        console.log(error.response.data)
    })

}
}
        //return
        return {
            barangs,
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
