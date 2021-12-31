<template>
	<div class="container mt-5"><div class="row">
		<div class="col-sm-12"><div class="card rounded border-0 shadow"><div class="card-body">
		<table class="table table-striped mt-4">
	<thead>
		<tr>
			<th>No</th>
			<th>Judul</th>
			<th>Konten</th>
		</tr>
	</thead>
	<tbody>
		<tr v-for="(post,index) in posts" :key="index">
			<td>{{index + 1}}</td>
			<td>{{post.title}}</td>
			<td>{{post.content}}</td>
		</tr>
	</tbody>
</table></div></div></div></div></div></template>


<script>
	import axios from 'axios'
	import {onMounted, ref} from 'vue'

	export default{
		setup(){
			let posts = ref([])


			onMounted(() => {
				axios.get("http://localhost:8000/api/post")
				.then(response => {
					posts.value = response.data.data
				}).catch(error => {
					console.log(error.response.data)
				})


			})
			return{
				posts
			}
			}

			}
		
	
</script>



























// import axios from 'axios'
	// import {onMounted,ref} from 'vue'
	// export default{
	// 	setup(){
	// 		let posts = ref([])

	// 		onMounted(() => {
	// 			axios.get("http://localhost:8000/api/post")
	// 			.then(response => {
	// 				posts.value = response.data.data
	// 			}).catch(error=>{
	// 				console.log(error.response.data)
	// 			})

	// 		})
	// 		return {
	// 			posts
	// 		}
	// 	}
	// }