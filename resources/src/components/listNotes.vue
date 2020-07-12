<template>
	<div class="listNotes">
		<ul>
			<li v-for="(row, index) in notes" :key="index">
				<button class="btn-note" @click="editNote(row.id)">
					<label>{{row.title}}</label>
					<span>{{row.description}}</span>
				</button>
			</li>
		</ul>
	</div>
</template>

<script type="text/javascript">
	
	export default {
		name: 'listNotes',
		data: function() {
			return {
				notes: [{ id:1, title: 'Wegodev',description: 'Ini isi wegodev' },{ id:2, title: 'Super Userz',description: 'Ini isi super user cuy' }]
			}
		},
		props: {
			propEditNote: {
				type: Function
			}
		},
		methods: {
			editNote(id){
				// console.log('App vue :' + id);
				let dataForm = this.notes.find(note => note.id === id);
				// console.log(this.dataForm);
				this.$root.$emit('emitForm', dataForm); //berkomunikasi antara listNote dan formNote. Jadi, dengan ini dapat mengirimkan sebuah event yang dapat ditangkap di dalam app.vue, formNote atau komponen lainnya.
			}
		},
		mounted() {
			this.$root.$on('emitRemoveNote', data => {
				let noteIndex = this.notes.findIndex(note => note.id === data.id);
				this.notes.splice(noteIndex, 1); // splice digunakan untuk membuang sebuah array berdasarkan nilai indexnya (yang dipilih user/ "1").
			});
			this.$root.$on('emitUpdateNote', data => {
				let noteIndex = this.notes.findIndex(note => note.id === data.id);    
				this.notes[noteIndex].title = data.title;
				this.notes[noteIndex].description = data.description;
			});
		}
	}

</script>

<style type="text/css">
	ul {
     list-style-type: none;
     padding: 0;
     margin:0px;
}
.btn-note {
     text-align: left;
     border: none;
     border-bottom: 1px solid gainsboro;
     padding: 0px 15px;
     cursor: pointer;
     outline: none;
     background: #f7f7f7;
     height: 150px;
     width: 100%;
}
.btn-note:hover {
     background:#eaeaea;
}
.btn-note label{
     display:block;
     color: #444444;
     font-size: 1.5em;
     margin-bottom: 15px;
}
.btn-note span{
    color: #545454;
}
</style>