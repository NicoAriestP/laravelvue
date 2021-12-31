<template>
    <div class="overflow-auto">
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <div class="card border-0 rounded shadow">
                    <div class="card-body">
                        <h4>EDIT POST</h4>
                        <hr>

                        <form @submit.prevent="update">
                            <div class="form-group mb-2">
                                <label for="title" class="font-weight-bold">NAMA</label>
                                <input type="text" class="form-control" v-model="barang.nama_barang" placeholder="Masukkan Nama Barang">
                                <!-- validation -->
                                <div v-if="validation.nama_barang" class="mt-2 alert alert-danger">
                                    {{ validation.nama_barang[0] }}
                                </div>
                            </div>
                            <div class="form-group mb-2">
                                <label for="content" class="font-weight-bold">HARGA</label>
                                <input type="text" class="form-control" rows="4" v-model="barang.harga" placeholder="Masukkan Harga Barang">
                                <!-- validation -->
                                <div v-if="validation.harga" class="mt-2 alert alert-danger">
                                    {{ validation.harga[0] }}
                                </div>
                            </div>
                            <div class="form-group mb-2">
                                <label for="content" class="font-weight-bold">STOK</label>
                                <input type="number" class="form-control" rows="4" v-model="barang.stok" placeholder="Masukkan Stok Barang">
                                <!-- validation -->
                                <div v-if="validation.stok" class="mt-2 alert alert-danger">
                                    {{ validation.stok[0] }}
                                </div>
                            </div>
                            <button type="submit" class="btn btn-success btn-sm">SIMPAN</button>
                        </form>                        

                    </div>
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

        //state posts
        const barang = reactive({
            nama_barang: '',
            harga: '',
            stok:''
        })

        //state validation
        const validation = ref([])

        //vue router
        const router = useRouter()

        //vue router
        const route = useRoute()

        //mounted
        onMounted(() => {

            //get API from Laravel Backend
            axios.get(`http://localhost:8000/api/barang/${route.params.id}`)
            .then(response => {
              
              //assign state posts with response data
              barang.nama_barang    = response.data.barang.nama_barang  
              barang.harga  = response.data.barang.harga  
              barang.stok  = response.data.barang.stok  

            }).catch(error => {
                console.log(error.response.data)
            })

        })

        //method update
        function update() {

            let nama_barang   = barang.nama_barang
            let harga = barang.harga
            let stok = barang.stok

            axios.put(`http://localhost:8000/api/barang/${route.params.id}`, {
                nama_barang: nama_barang,
                harga: harga,
                stok:stok
            }).then(() => {

                //redirect ke post index
                router.push({
                    name: 'post.index'
                })

            }).catch(error => {
                //assign state validation with error 
                validation.value = error.response.data
            })

        }

        //return
        return {
            barang,
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