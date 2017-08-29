<template>
	<div class="table">
		<div class="crumbs">
			<el-breadcrumb separator="/">
				<el-breadcrumb-item><i class="el-icon-menu"></i> 订单管理</el-breadcrumb-item>
				<el-breadcrumb-item> 订单列表</el-breadcrumb-item>
			</el-breadcrumb>
		</div>

		<div class="handle-box">
			<el-button type="warning" icon="delete" class="handle-del mr10" @click="delAll">批量删除</el-button>
			<span style='margin-left:30px;margin-right:10px'>手机号:</span>
			<el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10" width='50'></el-input>
			<span style='margin-left:30px;margin-right:10px'>订单号:</span>
			<el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10" width='50'></el-input>
			<span style='margin-left:30px;margin-right:10px'>下单时期:</span>
			<el-input v-model="select_word" placeholder="筛选关键词" class="handle-input mr10" width='50'></el-input>
			<el-button type="primary" icon="search" @click="search" style='float:right'>搜索</el-button>

		</div>
		<el-table :data="tableData" border style="width: 100%" ref="multipleTable" @selection-change="handleSelectionChange">
			<el-table-column type="selection" width="55"></el-table-column>
			<el-table-column prop="id" label="订单id" sortable width="100">
			</el-table-column>
			<el-table-column prop="name" label="会员" width="135">
			</el-table-column>

			<el-table-column prop="name" label="收货信息">

			</el-table-column>
			<el-table-column prop="name" label="下单时间">
			</el-table-column>
			<el-table-column prop="name" label="总金额">
			</el-table-column>
			<el-table-column prop="name" label="支付方式">
			</el-table-column>
			<el-table-column prop="name" label="配送方式">
			</el-table-column>
			<el-table-column prop="name" label="订单状态">
			</el-table-column>
			<el-table-column label="操作" width="180">
				<template scope="scope">
					<router-link to="/orderDetail">
						<el-button size="small" type="primary">查看</el-button>
					</router-link>
					<el-button size="small" type="warning" @click="handleDelete(scope.$index, scope.row)">操作</el-button>
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
	export default {
		data() {
			return {
				activeName: 'first',
				url: './static/vuetable.json',
				tableData: [{
						"id": "1",
						"name": "蓝晴Fontanesia",
						"type": "小西点",
						"price": '268',
						"describe": "纯动物淡奶油、乳酪威风蛋糕，新鲜事领水果，水果混合草莓夹心",
						"buyNumber": "28",
						"pos": "#1234",
						"ifHot": "0",
						"ifTop": "0"
					},
					{
						"id": "1",
						"name": "蓝晴Fontanesia",
						"type": "小西点",
						"price": '268',
						"describe": "纯动物淡奶油、乳酪威风蛋糕，新鲜事领水果，水果混合草莓夹心",
						"buyNumber": "28",
						"pos": "#1234",
						"ifHot": "0",
						"ifTop": "0"
					},
					{
						"id": "1",
						"name": "蓝晴Fontanesia",
						"type": "小西点",
						"price": '268',
						"describe": "纯动物淡奶油、乳酪威风蛋糕，新鲜事领水果，水果混合草莓夹心",
						"buyNumber": "28",
						"pos": "#1234",
						"ifHot": "0",
						"ifTop": "0"
					},
					{
						"id": "1",
						"name": "蓝晴Fontanesia",
						"type": "小西点",
						"price": '268',
						"describe": "纯动物淡奶油、乳酪威风蛋糕，新鲜事领水果，水果混合草莓夹心",
						"buyNumber": "28",
						"pos": "#1234",
						"ifHot": "0",
						"ifTop": "0"
					},
					{
						"id": "1",
						"name": "蓝晴Fontanesia",
						"type": "小西点",
						"price": '268',
						"describe": "纯动物淡奶油、乳酪威风蛋糕，新鲜事领水果，水果混合草莓夹心",
						"buyNumber": "28",
						"pos": "#1234",
						"ifHot": "0",
						"ifTop": "0"
					},
					{
						"id": "1",
						"name": "蓝晴Fontanesia",
						"type": "小西点",
						"price": '268',
						"describe": "纯动物淡奶油、乳酪威风蛋糕，新鲜事领水果，水果混合草莓夹心",
						"buyNumber": "28",
						"pos": "#1234",
						"ifHot": "0",
						"ifTop": "0"
					},
					{
						"id": "1",
						"name": "蓝晴Fontanesia",
						"type": "小西点",
						"price": '268',
						"describe": "纯动物淡奶油、乳酪威风蛋糕，新鲜事领水果，水果混合草莓夹心",
						"buyNumber": "28",
						"pos": "#1234",
						"ifHot": "0",
						"ifTop": "0"
					},
					{
						"id": "1",
						"name": "蓝晴Fontanesia",
						"type": "小西点",
						"price": '268',
						"describe": "纯动物淡奶油、乳酪威风蛋糕，新鲜事领水果，水果混合草莓夹心",
						"buyNumber": "28",
						"pos": "#1234",
						"ifHot": "0",
						"ifTop": "0"
					}
				],
				cur_page: 1,
				multipleSelection: [],
				select_cate: '',
				select_word: '',
				del_list: [],
				is_search: false
			}
		},
		created() {
			this.getData();
		},
		mounted: function() {
			let type = localStorage.getItem('type');

		},
		computed: {
			data() {
				const self = this;
				return self.tableData.filter(function(d) {
					let is_del = false;
					for(let i = 0; i < self.del_list.length; i++) {
						if(d.name === self.del_list[i].name) {
							is_del = true;
							break;
						}
					}
					// if(!is_del){
					//     if(d.address.indexOf(self.select_cate) > -1 && 
					//         (d.name.indexOf(self.select_word) > -1 ||
					//         d.address.indexOf(self.select_word) > -1)
					//     ){
					//         return d;
					//     }
					// }
				})
			}
		},
		methods: {
			handleCurrentChange(val) {
				this.cur_page = val;
				this.getData();
			},
			getData() {
				let self = this;
				if(process.env.NODE_ENV === 'development') {
					self.url = '/ms/table/list';
				};
				// self.$axios.post(self.url, {page:self.cur_page}).then((res) => {
				//     console.log(JSON.stringify(res.data.list))
				//     self.tableData = res.data.list;
				// })
			},
			search() {
				this.is_search = true;
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
</style>