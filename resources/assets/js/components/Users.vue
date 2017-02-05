<template>
	<div>
		<table class="table table-striped">
			<thead>
				<tr>
					<th>Photo</th>
					<th>Username</th>
					<th>Email</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="user in users">
					<td><img :src="user.photo ? user.photo.file : '/images/default-profile.png'"></td>
					<td>{{user.name}}</td>
					<td>{{user.email}}</td>
				</tr>
			
			</tbody>
		</table>
	</div>
</template>

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