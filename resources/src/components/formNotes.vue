<template>
	<div class="formNotes">

		<form>
		<div class="menu">
			<button type="button" @click="submitRemove" class="bg-danger btn btn-delete">Delete</button>
			<button type="button" @click="submitSave" class="bg-success btn" v-if = "mode == 'save'">Save</button>
			<button type="button" @click="submitUpdate" class="bg-success btn" v-if = "mode == 'update'">Update</button>
		</div>

		<div class="content">
			<input type="text" class="text" placeholder="ID" v-model="id">
			<input type="text" class="text" placeholder="Title" v-model="title">
			<textarea class="text textarea" placeholder="Tuliskan rencana kamu..." v-model="description"></textarea>
		</div>
		</form>
	</div>
</template>

<script type="text/javascript">
	import axios from 'axios';

	export default {
		name: 'formNotes',
		// props: {
		// 	propSaveNote : {
		// 		type: Function
		// 	}
		// 	// propUpdateNote : {
		// 	// 	type: Function
		// 	// },
		// 	// propRemoveNote : {
		// 	// 	type: Function
		// 	// }
		// 	// propDataForm : {
		// 	// 	type: Object
		// 	// }
		// },
		data: function() {
			return {
				id: 0,
				title: '',
				description: '',
				mode: 'save'
			}
		},
		methods: {
			submitSave() {

				let params = new URLSearchParams();
				params.append('title', this.title);
				params.append('description', this.description);

				axios.post('/api/notes', params).then(response =>{
					let data = {
						id: response.data.id,
						title: this.title,
						description: this.description
					}
					this.$root.$emit('emitSaveNote', data);
					console.log(response.data);
				});

			},

			submitUpdate() {
				let params = new URLSearchParams();
				params.append('id', this.id);
				params.append('title', this.title);
				params.append('description', this.description);

				axios.put('/api/notes/'+this.id, params).then(response =>{
					let data = {
						id: response.data.id,
						title: this.title,
						description: this.description
					}
					this.$root.$emit('emitUpdateNote', data);
				});
			},

			submitRemove() {
				let params = new URLSearchParams();
				params.append('id', this.id);

				axios.delete('/api/notes/'+this.id, params).then(response =>{
					let data = {
						id: response.data.id
					}
					this.$root.$emit('emitRemoveNote', data);
					this.resetInput();
				});
			},
			resetInput() {
				this.id = 0;
				this.title = '';
				this.description = '';
			}
		},
		mounted(){  //mounted sangat cocok ketika kita mendeklarasikan hal2 yang harus dijalankan pertama kali dari sebuah komponen.
			this.$root.$on('emitForm', data => { //"$on" akan menjadi penerima dari emit yang ada dalam listNotes
				this.id = data.id;
				this.title = data.title;
				this.description = data.description;
				this.mode = data.mode;
			})  
		}
	}

</script>

<style type="text/css">
	.menu{
     background: #f7f7f7;
     padding:10px 25px;
     margin-bottom: 25px;
     text-align:right;
     border-bottom: 1px solid #e8e6e6;
}
.btn-delete{ margin-right:10px; }
.content{
     padding: 0px 25px;
}
.text{
     display: block;
     width: 100%;
     padding: 0px;
     font-size: 20px;
     font-weight: bold;
     color: #2c3e50;
     border: none;
     margin-bottom: 10px;
     box-sizing: border-box;
     outline: none;
}
.textarea{
     min-height: 350px;
     font-size: 15px;
     font-weight: lighter;
     line-height: 30px;
}
.loader{
     vertical-align: middle;
}
</style>