<template>
	<div id="form">
		<div class="crumbs">
			<el-breadcrumb separator="/">
				<el-breadcrumb-item><i class="el-icon-menu"></i> 兑换管理</el-breadcrumb-item>
				<el-breadcrumb-item :to="{ path: '/bigOrder' }"> 兑换列表</el-breadcrumb-item>
				<el-breadcrumb-item :to="{ path: '/bigOrderDetail' }"> 兑换详情</el-breadcrumb-item>
			</el-breadcrumb>
			<el-row :gutter="50">
				<el-col :span="8">基本信息</el-col>
			</el-row>
			<el-row :gutter="50">
				<el-table :data="tableData" border style="width: 100%" ref="multipleTable"  class="firstTable">
					<el-table-column prop="id" label="订单ID" width="100">
					</el-table-column>
					<el-table-column prop="out_trade_no" label="订单号" >
					</el-table-column>
					<el-table-column prop="address" label="会员">
						<template scope='scope'>
							<p >{{scope.row.address.split('/')[0]}}</p>
						</template>
					</el-table-column>
					<el-table-column prop="address" label="电话">
						<template scope='scope'>
							<p >{{scope.row.address.split('/')[1]}}</p>
						</template>
					</el-table-column>
					<el-table-column prop="actual_fee" label="实付">
						<template scope='scope'>
							<p >0</p>
						</template>
					</el-table-column>
					<el-table-column prop="status" label="订单状态">
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
					<el-table-column prop="created_at" label="下单时间">
					</el-table-column>
					<el-table-column prop="price" label="支付方式">
						<template scope='scope'>
							礼品卡支付
						</template>
					</el-table-column>
				</el-table>
			</el-row>
			<el-row :gutter="50">
				<el-col :span="8">收货信息</el-col>
			</el-row>
			<el-row :gutter="50">
				<el-table :data="tableData" border style="width: 100%" ref="multipleTable"  class="firstTable">
					<el-table-column prop="id" label="收货人">
						<template scope='scope'>
							<p >{{scope.row.address.split('/')[0]}}</p>
						</template>
					</el-table-column>
					<el-table-column prop="id" label="联系方式">
						<template scope='scope'>
							<p >{{scope.row.address.split('/')[1]}}</p>
						</template>
					</el-table-column>
					<el-table-column prop="name" label="地址">
						<template scope='scope'>
							<p >{{scope.row.address.split('/')[2]}}</p>
						</template>
					</el-table-column>
					<el-table-column prop="remark" label="备注信息">
					</el-table-column>
				</el-table>
			</el-row>
			<el-row :gutter="50">
				<el-col :span="8">商品信息</el-col>
			</el-row>
			<el-row :gutter="50">
				<el-table :data="goods" border style="width: 100%" ref="multipleTable"  class="firstTable">
					<el-table-column prop="name" label="商品">
					</el-table-column>
					<el-table-column prop="pivot.attribute" label="商品属性">
					</el-table-column>
					<el-table-column prop="pivot.number" label="数量">
					</el-table-column>
					<el-table-column prop="price" label="单价">
					</el-table-column>
					<el-table-column prop="" label="小计">
						<template scope='scope'>
							<p >{{scope.row.pivot.number*scope.row.price}}</p>
						</template>
					</el-table-column>
				</el-table>
			</el-row>
			
		</div>

	</div>
</template>

<script>
	import { quillEditor } from 'vue-quill-editor';
	import api from '../../api/api.json';
    import axios from 'axios';
    import qs from 'qs';
	export default {
		data() {
			return {
				
				tableData: [],
				goods:[],
				works:[],
				changestatus:"",
				remark:'',
		        options: [{
		          value: '',
		          label: '全部'
		        }, {
		          value: '0',
		          label: '待付款'
		        }, {
		          value: '1',
		          label: '已付款'
		        }, {
		          value: '2',
		          label: '配送中'
		        }, {
		          value: '3',
		          label: '配送完成'
		        }, {
		          value: '4',
		          label: '确认收货'
		        }, {
		          value: '5',
		          label: '申请退款'
		        }, {
		          value: '6',
		          label: '已退款'
		        }],

			}
		},
		components: {
			quillEditor
		},
		mounted:function(){
			this.getData()
		},
		methods: {
			getData(){

				let self = this;

                axios.get(api.baseUrl +'/bigOrder/'+self.$route.query.id,
                ).then((res) => {
                    if(res.data.responseCode == 0) {
                        self.$message({
                          type: 'info',
                          message: `网络异常，获取失败`
                        });
                    } else {
                    	self.tableData=[]
                    	self.tableData.push(res.data.data)
                    	self.goods=res.data.data.goods
                    	self.works = res.data.data.options
                    	self.changestatus=""+res.data.data.status
                    	//self.store=res.data.data.data
                        
                    }
                }).catch(function(error) {
                    //console.log(error);
                });
			},
			onSubmit() {
				let self = this;
                axios.post(api.baseUrl +'/bigOrder/modify/'+self.$route.query.id,
                	qs.stringify({
						status: self.changestatus,
						remark: self.remark
					})
                ).then((res) => {
                    if(res.data.responseCode == 0) {
                        self.$message({
                          type: 'info',
                          message: `网络异常，获取失败`
                        });
                    } else {
                    	self.$message({
                          type: 'info',
                          message: `操作成功`
                        });
                    	self.getData()
                    	//self.store=res.data.data.data
                        
                    }
                }).catch(function(error) {
                    //console.log(error);
                });
			}

		}

	}
</script>

<style>
	body {
		font-family: Helvetica, sans-serif;
	}
	
	.el-row {
		margin-bottom: 20px;
		&:last-child {
			margin-bottom: 0;
		}
	}
	
	.el-breadcrumb {
		margin-bottom: 30px
	}
	
	.avatar-uploader .el-upload {
		border: 1px dashed #d9d9d9;
		border-radius: 6px;
		cursor: pointer;
		position: relative;
		overflow: hidden;
		width: 180px;
	}
	
	.avatar-uploader .el-upload:hover {
		border-color: #20a0ff;
	}
	
	.avatar-uploader-icon {
		font-size: 28px;
		color: #8c939d;
		width: 178px;
		height: 178px;
		line-height: 178px;
		text-align: center;
	}
	
	.avatar {
		width: 100%;
		height: 100%;
		display: block;
	}


	.firstTable th{
		background: #c9e5f5
	}
	.firstTable th div{
		background: none

	}
	.secondTable th{
		background: #dff0d8
	}
	.secondTable th div{
		background: #dff0d8
	}
</style>