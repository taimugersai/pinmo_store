<template>
	<div class="table">
		<div class="crumbs">
			<el-breadcrumb separator="/">
				<el-breadcrumb-item><i class="el-icon-menu"></i> 商品管理</el-breadcrumb-item>
				<el-breadcrumb-item> 商品列表</el-breadcrumb-item>
			</el-breadcrumb>
		</div>

		<div class="handle-box">
			<div class="search_box">
				<el-button type="warning" icon="delete" class="handle-del mr10" @click="delAll">批量删除</el-button>
			</div>
			<div class="search_box">
				<span>商品搜索：</span>
				<el-input v-model="searchData.search" placeholder="请输入商品名称或编号" class="handle-input mr10"></el-input>
			</div>
			<div class="search_box">
				<span>分类选择：</span>
				<el-select v-model="searchData.category_id" placeholder="请选择">
					<el-option
				      label="全部"
				      value="">
				    </el-option>
				    <el-option
				      v-for="item in Category"
				      :label="item.name"
				      :value="item.id">
				    </el-option>
				  </el-select>
			</div>
			<div class="search_box">
				<el-button type="primary" icon="search" @click="goodsSearch">搜索</el-button>
			</div>
			<div class="search_box" style='float:right;margin-top:10px;'>
				<router-link to="/addGood">
					<el-button type="success" icon="edit" class="handle-edit mr10" >新增商品</el-button>
				</router-link>
			</div>
			
			
			
			
			
			
		</div>
		<el-table :data="tableData" border style="width: 100%" ref="multipleTable" @selection-change="handleSelectionChange">
			<el-table-column type="selection" width="55"></el-table-column>
			<el-table-column prop="id" label="序号" sortable width="100">
			</el-table-column>
			<el-table-column  label="商品名称" >
				<template scope="scope">
					<span>{{scope.row.name}}</span>
					<span class='iconfont' style="color:rgb(255, 102, 102)" v-if='scope.row.is_recommend'>&#xe600;</span>
					<span class='iconfont' style="color:rgb(245, 166, 35)" v-if='scope.row.is_top'>&#xe61e;</span>
				</template>
			</el-table-column>
			<el-table-column prop="category.name" label="所属分类" width="120">
				
			</el-table-column>
			<el-table-column prop="price" label="价格" width="120">
			</el-table-column>
			<!-- <el-table-column prop="desc" label="商品描述" >
			</el-table-column> -->
			<el-table-column prop="pos_no" label="对应pos" width="100">
			</el-table-column>
			<!-- <el-table-column prop="is_top" label="是否热门" width="100">
			</el-table-column>
			<el-table-column prop="is_recommend" label="是否置顶" width="100">
			</el-table-column> -->
			<el-table-column label="操作" width="180">
				<template scope="scope">
					<router-link :to="{ path: 'addGood', query: { id: scope.row.id}}" >
						<el-button size="small" type="primary">编辑</el-button>
					</router-link>
					<el-button size="small" type="danger" @click="handleDelete(scope.row)">删除</el-button>
				</template>
			</el-table-column>
		</el-table>
		<!-- 分页 -->
		<div class="pagination">
			<el-pagination @current-change="handleCurrentChange" layout="prev, pager, next" :total="total" :page-size='size'>
			</el-pagination>
		</div>
		</el-tab-pane>
	</div>
</template>

<script>
	import api from '../../api/api.json'
	import axios from 'axios';
	import qs from 'qs';
	export default {
		data() {
			return {
				tableData: [],
				cur_page: 1,
				multipleSelection: [],
				select_word: '',
				del_list: [],
				is_search: false,
				size:0,
                total:10,

                Category: [],

                searchData:{
                	search:"",
                	category_id:''
                },
		        searchCategoryId:'',
		        searchWord:''
			}
		},
		created() {
			this.getData();
			this.getAllCategory()
		},
		methods: {
			handleCurrentChange(val) {
				this.cur_page = val;
				this.getData();
			},
			goodsSearch(){
				let self = this;
                axios.get(api.baseUrl +'/goods/'+localStorage.getItem('type')+'?page=&search='+this.searchData.search+'&category_id='+this.searchData.category_id,
                ).then((res) => {
                    if(res.data.responseCode == 0) {
                        self.$message({
                          type: 'info',
                          message: `网络异常，获取失败`
                        });
                    } else {
                        self.tableData=res.data.data.data;
                        self.size=res.data.data.per_page;
                        self.total=res.data.data.total;
                    }
                }).catch(function(error) {
                    //console.log(error);
                });
			},
			getAllCategory(){
				let self = this;
                axios.get(api.baseUrl +'/category/all/'+localStorage.getItem('type'),
                ).then((res) => {
                    if(res.data.responseCode == 0) {
                        self.$message({
                          type: 'info',
                          message: `网络异常，获取失败`
                        });
                    } else {
                    	////console.log(res.data.data)
                        self.Category=res.data.data;
                        // self.size=res.data.data.per_page;
                        // self.total=res.data.data.total;
                    }
                }).catch(function(error) {
                    //console.log(error);
                });
			},
			getData() {
				let self = this;
                axios.get(api.baseUrl +'/goods/'+localStorage.getItem('type')+'?page='+self.cur_page,
                ).then((res) => {
                    if(res.data.responseCode == 0) {
                        self.$message({
                          type: 'info',
                          message: `网络异常，获取失败`
                        });
                    } else {
                        self.tableData=res.data.data.data;
                        self.size=res.data.data.per_page;
                        self.total=res.data.data.total;
                    }
                }).catch(function(error) {
                    //console.log(error);
                });
			},
			search() {
				this.is_search = true;
			},
			handleDelete(row) {
				let self = this;
				this.$confirm('确认删除？')
		          .then(_ => {
		            axios.post(api.baseUrl + '/goods/'+'destroy/'+row.id,
	                    qs.stringify({})
	                ).then((res) => {
	                    if(res.data.responseCode == 0) {
	                        self.$message({
	                          type: 'info',
	                          message: `网络异常，删除失败`
	                        });
	                    } else {
	                        self.$message({
	                          type: 'info',
	                          message: `删除成功`
	                        });
	                        self.multipleSelection = [];
	                        self.getData()
	                    }
	                }).catch(function(error) {
	                    //console.log(error);
	                });
		          })
		          .catch(_ => {});
                
			},
			delAll() {
				const self = this,
					length = self.multipleSelection.length;
				let str =[];
				self.del_list = self.del_list.concat(self.multipleSelection);
				if(length==0){

                    self.$message({
                      type: 'info',
                      message: `请选择待删除行数`
                    });
                }else{
                	for(let i = 0; i < length; i++) {
						str.push(self.multipleSelection[i].id);
					}
					var row={
	                    id:str.join(',')
	                }
	                self.handleDelete(row)
					
                }
				
			},
			handleSelectionChange(val) {
				this.multipleSelection = val;
			}
		}
	}
</script>
<style scoped>
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

	@font-face {
	  font-family: 'iconfont';  /* project id 402141 */
	  src: url('https://at.alicdn.com/t/font_402141_g71wq40q95krcnmi.eot');
	  src: url('https://at.alicdn.com/t/font_402141_g71wq40q95krcnmi.eot?#iefix') format('embedded-opentype'),
	  url('https://at.alicdn.com/t/font_402141_g71wq40q95krcnmi.woff') format('woff'),
	  url('https://at.alicdn.com/t/font_402141_g71wq40q95krcnmi.ttf') format('truetype'),
	  url('https://at.alicdn.com/t/font_402141_g71wq40q95krcnmi.svg#iconfont') format('svg');
	}
	.iconfont{
		font-family: 'iconfont';
		font-size: 22px;
	}
	.search_box{
		display: inline-block;
		margin-top: 10px;
	}
</style>