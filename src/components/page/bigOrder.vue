<template>
	<div class="table">
		<!-- <div class="crumbs">
			<el-breadcrumb separator="/">
				<el-breadcrumb-item><i class="el-icon-menu"></i> 订单管理</el-breadcrumb-item>
				<el-breadcrumb-item> 订单列表</el-breadcrumb-item>
			</el-breadcrumb>
		</div> -->

		<div class="handle-box">
			<div class="search_box">
				<span style='margin-left:10px;margin-right:10px'>订单:</span>
				<el-input v-model="out_trade_no" placeholder="输入订单号" class="handle-input mr10" style='width:200px'></el-input>
			</div>
			<div class="search_box">
				<span style='margin-left:10px;margin-right:10px'>选择时间:</span>
			     <el-date-picker
			      v-model="ordered_at"
			      type="daterange"
			      align="right"
			      unlink-panels
			      range-separator="至"
			      start-placeholder="开始日期"
			      end-placeholder="结束日期"
			      value-format='yyyy-MM-dd'

			      :picker-options="pickerOptions2">
			    </el-date-picker>
			</div>

			<div class="search_box">
				<span style='margin-left:10px;margin-right:10px'>订单状态:</span>
			    <el-select v-model="status" placeholder="请选择">
				    <el-option
				      v-for="item in options"
				      :key="item.value"
				      :label="item.label"
				      :value="item.value">
				    </el-option>
				</el-select>
			</div>
			
			<!-- <div class="search_box">
				<span style='margin-left:10px;margin-right:10px'>门店:</span>
				<el-select v-model="store_id" placeholder="请选择">
					<el-option
				      
				      label="全部 "
				      value="">
				    </el-option>
				    <el-option
				      v-for="item in store"
				      :key="item.value"
				      :label="item.display_name "
				      :value="item.id">
				    </el-option>
				  </el-select>
			</div> -->
			
			<div class="search_box">
				<el-button type="primary" icon="search" @click="search" style='margin-top:10px;margin-left:20px;'>搜索</el-button>
			</div>
		</div>
		<el-table :data="tableData" border style="width: 100%" ref="multipleTable" @selection-change="handleSelectionChange">
			<el-table-column type="selection" width="55"></el-table-column>
			<el-table-column prop="out_trade_no" label="订单号" sortable width="200">
			</el-table-column>
			<el-table-column prop="goods" label="商品信息">
			</el-table-column>
			<el-table-column prop="address" label="收货信息">
			</el-table-column>
			<el-table-column prop="delivery" label="配送方式" width="100">
			</el-table-column>
			<el-table-column prop="store" label="门店名称">
			</el-table-column>
			
			<el-table-column prop="name" label="订单状态" width="125">
				<template scope='scope'>
					<p v-if='scope.row.status==0' style="color:#F7BA2A">待付款</p>
					<p v-if='scope.row.status==1' style="color:#20A0FF">已付款</p>
					<p v-if='scope.row.status==2' style="color:#20A0FF">配送中</p>
					<p v-if='scope.row.status==3' style="color:#13CE66">配送完成</p>
					<p v-if='scope.row.status==4' style="color:#13CE66">确认收货</p>
					<p v-if='scope.row.status==5' style="color:#F7BA2A">申请退款</p>
					<p v-if='scope.row.status==6' style="color:#FF4949">已退款</p>
					
				</template>
			</el-table-column>
			<el-table-column label="操作" width="100">
				<template scope="scope">
					<router-link   :to="{ path: 'bigOrderDetail', query: { id: scope.row.id}}" >
						<el-button size="small" type="primary">操作</el-button>
					</router-link>
					
				</template>
			</el-table-column>
		</el-table>
		<div class="pagination">
			<el-pagination @current-change="handleCurrentChange" layout="prev, pager, next" :total="10">
			</el-pagination>
		</div>
		</el-tab-pane>
	</div>

	</div>
</template>

<script>
	import api from '../../api/api.json';
    import axios from 'axios';
    import qs from 'qs';
	export default {
		data() {
			return {
				activeName: 'first',
				url: './static/vuetable.json',
				tableData: [],
				cur_page: 1,
				multipleSelection: [],
				select_cate: '',
				select_word: '',
				del_list: [],
				is_search: false,


				pickerOptions1: {
		          shortcuts: [{
		            text: '今天',
		            onClick(picker) {
		              picker.$emit('pick', new Date());
		            }
		          }, {
		            text: '昨天',
		            onClick(picker) {
		              const date = new Date();
		              date.setTime(date.getTime() - 3600 * 1000 * 24);
		              picker.$emit('pick', date);
		            }
		          }, {
		            text: '一周前',
		            onClick(picker) {
		              const date = new Date();
		              date.setTime(date.getTime() - 3600 * 1000 * 24 * 7);
		              picker.$emit('pick', date);
		            }
		          }]
		        },


				options: [{
		          value: 'all',
		          label: '全部'
		        }, {
		          value: '0',
		          label: '待付款'
		        }, {
		          value: '1',
		          label: '已付款'
		        }, {
		          value: '2',
		          label: '已接单'
		        },  {
		          value: '3',
		          label: '配送中'
		        }, {
		          value: '4',
		          label: '配送完成'
		        }, {
		          value: '5',
		          label: '确认收货'
		        }, {
		          value: '6',
		          label: '申请退款'
		        }, {
		          value: '7',
		          label: '已退款'
		        }],
		        store:[],


		        out_trade_no:'',
		        ordered_at:'',
		        status:"all",
		        store_id:''
			}
		},
		created() {
			if(this.$router.history.current.query.status){
				this.status=this.$router.history.current.query.status
			}
			this.getData();
			//this.getStore();
		},
		methods: {
			handleCurrentChange(val) {
				this.cur_page = val;
				this.getData();
			},
			getStore(){
				let self = this;
                axios.get(api.baseUrl +'/stores',
                ).then((res) => {
                    if(res.data.responseCode == 0) {
                        self.$message({
                          type: 'info',
                          message: `网络异常，获取失败`
                        });
                    } else {
                    	//console.log(res)
                    	self.store=res.data.data
                        
                    }
                }).catch(function(error) {
                    //console.log(error);
                });
			},
			getData() {
				let self = this;
                axios.get(api.baseUrl +'/bigOrders',
                	{
					    params: {
					      	page:self.cur_page
					    }
					}
                ).then((res) => {
                    if(res.data.responseCode == 0) {
                        self.$message({
                          type: 'info',
                          message: `网络异常，获取失败`
                        });
                    } else {
                        self.tableData=res.data.data.data
                        self.size=res.data.data.per_page
                        self.total=res.data.data.total
                    }
                }).catch(function(error) {
                    //console.log(error);
                });
			},
			search() {
				let self = this;
				var data={}
				var array = String(self.ordered_at)
				if(array.split(',')[1]){
					data={
				    	page:0,
				      	date_range:array.split(',')[0]+'/'+array.split(',')[1],
						status: self.status
				    }
				}else{
					data={
				    	page:0,
						status: self.status
				    }
				}
                axios.get(api.baseUrl +'/bigOrders',
                	 {
					    params: data
					  }
                ).then((res) => {
                    if(res.data.responseCode == 0) {
                        self.$message({
                          type: 'info',
                          message: `网络异常，获取失败`
                        });
                    } else {
                        self.tableData=res.data.data.data
                        self.size=res.data.data.per_page
                        self.total=res.data.data.total
                    }
                }).catch(function(error) {
                    //console.log(error);
                });
			},
			formatter(row, column) {
				return row.address;
			},
			filterTag(value, row) {
				return row.tag === value;
			},
			handleEdit(index, row) {
				this.$message('编辑第' + (index + 1) + '行');
			},
			handleDelete(index, row) {
				this.$message.error('删除第' + (index + 1) + '行');
			},
			delAll() {
				const self = this,
					length = self.multipleSelection.length;
				let str = '';
				self.del_list = self.del_list.concat(self.multipleSelection);
				for(let i = 0; i < length; i++) {
					str += self.multipleSelection[i].name + ' ';
				}
				self.$message.error('删除了' + str);
				self.multipleSelection = [];
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

	.search_box{
		display: inline-block;
		margin-top: 10px;
	}
</style>