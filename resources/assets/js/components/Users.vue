<template>
	<div>
		<table class="table table-striped">
			<thead>
				<tr>
					<th>Photo</th>
					<th>Username</th>
					<th>Email</th>
					<th>Edit</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="user in users" >
					<td><img :src="user.photo ? user.photo.file : '/images/default-profile.png'"></td>
					<td>{{user.name}}</td>
					<td>{{user.email}}</td>
					<td><router-link :to="{ path: 'user/'+user.id+'/edit' }" class="btn btn-primary" >Edit</router-link></td>
					<!-- <td><button class="btn-primary" @click="redirectToPage('/user/', category.id)">Edit</button></td> -->
				</tr>
			
			</tbody>
		</table>
	</div>
</template>
<style type="text/css">
	img{
		width: 100px;
		margin: auto;
		display: block;
		margin-bottom: 10px;
	}
</style>

<script>
	export default{
		data(){
			return{
				users:[]
			}
		},
		created(){
			this.fetchUsers();
		},
		methods:{
			fetchUsers(){
				this.$http.get('api/users').then(response=>{
					this.users = response.data.users;
				})
			}
		}
	}
</script>