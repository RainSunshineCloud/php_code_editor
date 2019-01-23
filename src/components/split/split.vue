<template>
	<div style="height:100%">
		<div style="height:4%;padding-top:5px;background:#2b303b;border-bottom: 1px solid #fff">
			<button class="btn-success" @click="send">执行</button>
			<button class ="btn-info" @click="copy" id="copy">复制</button>
			<button class ="btn-danger" @click="clear">清空</button>
		</div>
		<splitpanes class="default-theme" style="height: 95%;" id="split" watch-slots>
	  		<div style="width:100%;height:100%;">
	  			<codemirror style="width:100% ;height:100%;" v-model="code" ref="myCm" :options="cmOptions"></codemirror>
	  		</div>
	  		<div id="show_res">{{exec_res}}</div>
		</splitpanes>
	</div>
</template>

<script>
import splitpanes from 'splitpanes'
import 'splitpanes/dist/splitpanes.css'
import 'codemirror/lib/codemirror.css'
import 'codemirror/theme/monokai-sublime.css'
import 'codemirror/mode/php/php.js'
import { codemirror } from 'vue-codemirror'
import Clipboard  from 'clipboard'

export default {
	name:'split',
	components: { splitpanes,codemirror},
	data () {
	    return {
	      	code: '<?php \n',
	      	cmOptions: {
		        tabSize: 4,
		        indentUnit: 4,
		        mode: 'php',
		        theme: 'monokai-sublime',
		        lineNumbers: true,
		        line: true,

	      	}
	    }
	},
	props: {
		exec_res:{
			default: '',
		},
		notify: {
			type: Object,
			default: function () {
				return {
					error: function (e) {
						alert('复制失败');
					},
					success: function (e) {
						alert('复制成功');
					}
				}
			}
		}
	},
  	methods: {
	  	send () {
	  		let code = this.code.substring(6);
	  		this.$emit('send',code);
	  	},
	  	clear: function () {
	  		this.code = '<?php \n';
	  		this.$refs.myCm.cminstance.setValue(this.code);
	  	},
	  	copy: function () {
	  		let clipboard = new Clipboard('#copy',{
	  			text: () => {return this.code;}
	  		});
	  		clipboard.on('success',e =>{
				this.notify.success(e);
				clipboard.destroy() //使用destroy可以清楚缓存
			});
			clipboard.on('error',e =>{
				this.notify.error(e);
				clipboard.destroy()
			});
	  	},
  	}
}
</script>
	
}
</script>
<style>

#split .splitpanes__pane:first-child {
  background: #2b303b;
  justify-content: center;
  align-items: center;
  display: flex;
}
#split .splitpanes__pane:last-child {
  background: #383F4B;
  
  display: flex;
}

#show_res {
	padding:10px 4px;
}

#split.splitpanes--vertical > .splitpanes__splitter {
  max-width: 5px;
  background: #ccc;
  border: 1px solid #433;
}

#split .CodeMirror {
	height:100%;
}

button {
    text-rendering: auto;
    color: initial;
    letter-spacing: normal;
    word-spacing: normal;
    text-transform: none;
    text-indent: 0px;
    text-shadow: none;
    display: inline-block;
    text-align: start;
    margin: 0em;
    font: 400 11px system-ui;
    padding: 4px 12px;
    margin-bottom: 0;

}
.btn-success {
	color: #fff;
    text-shadow: 0 -1px 0 rgba(0,0,0,0.25);
    background-color: #5bb75b;
    background-image: linear-gradient(to bottom,#62c462,#51a351);
    background-repeat: repeat-x;
    border-color: #51a351 #51a351 #387038;
    border-color: rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);
}
.btn-danger {
    color: #fff;
    text-shadow: 0 -1px 0 rgba(0,0,0,0.25);
    background-color: #da4f49;
    background-image: linear-gradient(to bottom,#ee5f5b,#bd362f);
    background-repeat: repeat-x;
    border-color: #bd362f #bd362f #802420;
    border-color: rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);
}
    
.btn-info {
    color: #fff;
    text-shadow: 0 -1px 0 rgba(0,0,0,0.25);
    background-color: #49afcd;
    background-image: linear-gradient(to bottom,#5bc0de,#2f96b4);
    background-repeat: repeat-x;
    border-color: rgba(0,0,0,0.1) rgba(0,0,0,0.1) rgba(0,0,0,0.25);
}
</style>