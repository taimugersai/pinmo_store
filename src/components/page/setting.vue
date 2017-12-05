<template>
	<div class="table">
		<el-form ref="form" :model="form" label-width="180px">
		  <el-form-item label="营业起始时间">
		    <el-col :span="11">
		      <el-time-select
				  v-model="value1"
				  :picker-options="{
				    start: '08:30',
				    step: '00:15',
				    end: '18:30'
				  }"
				  placeholder="选择时间">
				</el-time-select>
		    </el-col>
		  </el-form-item>
		  <el-form-item label="营业结束时间">
		    <el-time-select
			  v-model="value1"
			  :picker-options="{
			    start: '08:30',
			    step: '00:15',
			    end: '18:30'
			  }"
			  placeholder="选择时间">
			</el-time-select>
		  </el-form-item>
		  <el-form-item>
		    <el-button type="primary" @click="onSubmit">立即创建</el-button>
		    <el-button>取消</el-button>
		  </el-form-item>
		</el-form>
	</div>

</template>

<script>
	import api from '../../api/api.json';
	import axios from 'axios';
	import qs from 'qs';
	export default {
		data() {
			return {
				form: {
		          
		          date1: '',
		          date2: '',
		         
		        },
		         value1: ''
			}
		},
		mounted() {
			var self = this
			
		},
		methods: {
			
		}

	}
</script>
<style scoped>
	.el-row {
		margin-bottom: 20px;
		&:last-child {
			margin-bottom: 0;
		}
	}
	
	.el-col {
		border-radius: 4px;
	}
	
	.bg-purple-dark {
		background: #f7ecb5;
		color: #8a6d3b;
	}
	
	.bg-purple {
		background: #e4b9b9;
		color: #a94442;
	}
	
	.bg-purple-light {
		background: #c1e2b3;
		color: #3c763d;
	}
	
	
	
	.row-bg {
		padding: 10px 0;
		background-color: #f9fafc;
	}
	/*标题*/
	
	.h1 {
		font-size: 36px;
		padding-bottom: 9px;
		margin: 40px 0 20px;
		border-bottom: 1px solid #eee;
	}
	
	.title {
		margin-top: 20px;
		font-size: 30px;
		color: #337ab7
	}
	/*内容*/
	
	.contents {
		margin-top: 20px;
	}
	
	.contents .el-col {
		text-align: center;
	}
	
	.contents .number {
		font-size: 18px;
		line-height: 80px;
		color: #337ab7;
	}
	.grid-content {
    width: 150px;
    height: 150px;
    border-radius: 100%;
    min-height: 36px;
    margin: 50px auto;
    filter: Alpha(Opacity=80);
    opacity: 0.8;
    position: relative;
    cursor: pointer;
  }
	.grid-content:hover {
		filter: Alpha(Opacity=100);
		opacity: 1;
	}
	/*搜索框*/
	
	.handle-box {
		margin-bottom: 20px;
	}
	
	.handle-select {
		width: 120px;
	}
	
	.handle-input {
		width: 300px;
		display: inline-block;
	}
	
	.search_box {
		display: inline-block;
		margin-top: 10px;
	}
	
	.el-icon-bell {
		color: white;
		background: red;
		padding: 10px;
		border-radius: 100%;
		position: absolute;
		top: 0;
		right: 0;
	}
</style>