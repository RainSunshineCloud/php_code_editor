<template>
    <div style="height: 100%">
        <split @send="send" :exec_res="exec_res"></split>
    </div>
</template>

<script>
import  split from '@/components/split';
import axios from 'axios';

export default {
    name: 'App',
    components:{
        split
    },
    data () {
    	return {
    		exec_res: '',
    	}
    },
    methods : {
    	send (code) {
    		console.log(code)
        	axios.post('/index.php',{
        			code
        		},{
        		headers: {
					'Content-Type':'application/x-www-form-urlencoded',
				},

				transformRequest: [
				function (data) {
			        let ret = ''
			        for (let it in data) {
			          ret += encodeURIComponent(it) + '=' + encodeURIComponent(data[it]) + '&'
			        }
			        return ret
      			}],
			
		}).then((response) => {
			   console.log(response);
        		this.exec_res = response.data;
        	}).catch(function (err) {
        		console.log(err)
        	});
		}
    }
}
</script>

<style>
</style>
