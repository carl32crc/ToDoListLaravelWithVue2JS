<template>
	<div>
		<h1>Edit User</h1>
		<form @submit.prevent="updateForm" enctype="multipart/form-data">
			<div class="form-group">
				<input class="form-control title" type="text" name="name" placeholder="name" v-model="user.name">
			</div>
			<div class="form-group">
				<input class="form-control title" type="text" name="email" placeholder="email" v-model="user.email">
			</div>
			<div class="form-group">
				<input class="form-control title" type="password" name="password" placeholder="password" >
			</div>
			<div class="form-group">
				<div v-if="!image">
					<h2>Select an image</h2>
				</div>
				<div v-else>
					<img :src="image" />
					<button @click="removeImage">Remove Image</button>
				</div>
				<input name="image" type="file" @change="onFileChange">
			</div>
			<button class="btn btn-primary" type="submit">Edit User</button>
		</form>
	</div>
</template>
<style>
	img{
		width: 30%;
		margin: auto;
		display: block;
		margin-bottom: 10px;
	}
</style>
<script>

	export default{
		data(){
			return {
				image:'',
				user:{}
			}
		},
		created(){
			this.fetchTask(this.$route.params.id);
		},
		methods:{
			fetchTask:function(id){
				this.$http.get('api/users/'+id).then(function(response){
					this.user = response.data;
				});
			},
			onFileChange(e){
				var files = e.target.files || e.dataTransfer.files;
				if(!files.length){
					return this.createImage(files[0]);
				}
			},
			createImage(file){
				var image = new Image();
				var reader = new FileReader();
				var vm = this;

				reader.onload = (e) => {
					vm.image = e.target.result;
				}
				reader.readAsDataURL(file);
			},
			removeImage: function (e){
				this.image = '';
			},
			updateForm:function(){
				var form = document.querySelector('form');
				var formdata = new FormData(form);

				this.$http.post('api/users/' + this.$route.params.id , formdata).then((response) =>{
					this.$router.push({path:'/', query:{alert:response.message}},(response)=>{
						console.log('error callback');
					});
				})
			}
		}
	}
</script>