<template>
  <div>
  <div id="app" class="">
    <pre>{{code}}</pre>
  </div>
    <div v-html="styleCode">
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  components: {
  },
  data () {
  	return{
  		code: ``,
      finalCode:
        `
/*大家好我是xxx*/
  		body{
  		  background: #3333;
  		}
  		  `
  	}
  },
  created () {
  	let n = 0;
    setInterval(()=>{
  		this.code = this.finalCode.substring(0,n);
  		n += 1
    },100)
  },
  computed:{
  	styleCode(){
  		return  '<style>'+ this.code + '<style>'
    }
  }
}
</script>

<style>

</style>
