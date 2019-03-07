<template>
  <div style="">
  	<button @click="JC">yuanxingJC</button>
  </div>
</template>




<script>
export default{
	data(){
		return{

		}
	},

	created:function(){

	},

	methods:{
		JC(){
			console.log('hello world!');
			let fa = new this.father();
			console.log(fa.name);
			fa.word;
		},

		father(){
			// let _this = this;
			this.name = 'Father';
			this.word = function(){
				console.log(_this.name+'is sleeping!')
			}
		}	
	}
}
</script>


