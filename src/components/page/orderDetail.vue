<template>
	<div id="form">
		<div class="crumbs">
			<el-breadcrumb separator="/">
				<el-breadcrumb-item><i class="el-icon-menu"></i> 订单管理</el-breadcrumb-item>
				<el-breadcrumb-item :to="{ path: '/order' }"> 订单列表</el-breadcrumb-item>
				<el-breadcrumb-item :to="{ path: '/orderDetail' }"> 订单详情</el-breadcrumb-item>
			</el-breadcrumb>
			<el-row :gutter="50">
				<el-col :span="8">订单明细</el-col>
			</el-row>
			<el-row :gutter="50">
				<el-col :span="8">
					<div>
						<el-form ref="form" :model="form1" label-width="90px">
							<div v-for="item in form1">
								<!-- 单行输入框 -->
								<el-form-item :label="item.label" v-if='item.type=="text"'>
									<el-input v-model="item.value"></el-input>
								</el-form-item>
								<!-- textarea -->
								<el-form-item :label="item.label" v-if='item.type=="textarea"'>
									<el-input type="textarea" :rows="2" :autosize="{ minRows: 2, maxRows: 5}" placeholder="请输入内容" v-model="item.value">
									</el-input>
								</el-form-item>
								<!-- 单选框 -->
								<el-form-item :label="item.label" v-if='item.type=="radio"'>
									<el-radio-group v-model="item.value">
										<el-radio :label="radio.value" v-for="radio in item.opinion"></el-radio>
									</el-radio-group>
								</el-form-item>
								<!-- 多选框 -->
								<el-form-item :label="item.label" v-if='item.type=="checkbox"'>
									<el-checkbox-group v-model="item.value">
										<el-checkbox :label="checkbox.value" name="type" v-for="checkbox in item.opinion"></el-checkbox>
									</el-checkbox-group>
								</el-form-item>
								<!-- 下拉列表 -->
								<el-form-item :label="item.label" v-if='item.type=="select"'>
									<el-select v-model="item.value" :placeholder="item.placeholder">
										<el-option :label="select.value" :value="select.value" v-for="select in item.opinion"></el-option>
									</el-select>
								</el-form-item>
								<!-- 开关 -->
								<el-form-item :label="item.label" v-if='item.type=="switch"'>
									<el-switch on-text="" off-text="" v-model="item.value"></el-switch>
								</el-form-item>
								<!-- 图片上传 -->
								<el-form-item :label="item.label" v-if='item.type=="imgUpload"'>
									<el-upload class="avatar-uploader" action="https://jsonplaceholder.typicode.com/posts/" :show-file-list="false" :on-success="handleAvatarSuccess" :on-preview="handlePictureCardPreview" :on-remove="handleRemove">
										<img v-if="imageUrl" :src="imageUrl" class="avatar">
										<i v-else class="el-icon-plus avatar-uploader-icon"></i>
									</el-upload>

								</el-form-item>
								<!-- 多图片上传 -->
								<el-form-item :label="item.label" v-if='item.type=="imgListUpload"'>
									<el-upload action="https://jsonplaceholder.typicode.com/posts/" list-type="picture-card" :on-preview="handlePictureCardPreview" :on-remove="handleRemove">
										<i class="el-icon-plus"></i>
									</el-upload>
									<el-dialog v-model="dialogVisible" size="tiny">
										<img width="100%" :src="dialogImageUrl" alt="">
									</el-dialog>

								</el-form-item>
								<!-- 富文本编辑器 -->
								<el-form-item :label="item.label" v-if='item.type=="editor"'>
									<quill-editor ref="myTextEditor" v-model="content" :config="editorOption"></quill-editor>
								</el-form-item>
								<!-- 提交 -->
								<el-form-item v-if='item.type=="submit"'>
									<el-button type="primary" @click="onSubmit">立即创建</el-button>
									<el-button>取消</el-button>
								</el-form-item>
							</div>
						</el-form>
					</div>
				</el-col>
				<el-col :span="8">
					<div>
						<el-form ref="form" :model="form2" label-width="80px">
							<div v-for="item in form2">
								<!-- 单行输入框 -->
								<el-form-item :label="item.label" v-if='item.type=="text"'>
									<el-input v-model="item.value"></el-input>
								</el-form-item>
								<!-- textarea -->
								<el-form-item :label="item.label" v-if='item.type=="textarea"'>
									<el-input type="textarea" :rows="2" :autosize="{ minRows: 2, maxRows: 5}" placeholder="请输入内容" v-model="item.value">
									</el-input>
								</el-form-item>
								<!-- 单选框 -->
								<el-form-item :label="item.label" v-if='item.type=="radio"'>
									<el-radio-group v-model="item.value">
										<el-radio :label="radio.value" v-for="radio in item.opinion"></el-radio>
									</el-radio-group>
								</el-form-item>
								<!-- 多选框 -->
								<el-form-item :label="item.label" v-if='item.type=="checkbox"'>
									<el-checkbox-group v-model="item.value">
										<el-checkbox :label="checkbox.value" name="type" v-for="checkbox in item.opinion"></el-checkbox>
									</el-checkbox-group>
								</el-form-item>
								<!-- 下拉列表 -->
								<el-form-item :label="item.label" v-if='item.type=="select"'>
									<el-select v-model="item.value" :placeholder="item.placeholder">
										<el-option :label="select.value" :value="select.value" v-for="select in item.opinion"></el-option>
									</el-select>
								</el-form-item>
								<!-- 开关 -->
								<el-form-item :label="item.label" v-if='item.type=="switch"'>
									<el-switch on-text="" off-text="" v-model="item.value"></el-switch>
								</el-form-item>
								<!-- 图片上传 -->
								<el-form-item :label="item.label" v-if='item.type=="imgUpload"'>
									<el-upload class="avatar-uploader" action="https://jsonplaceholder.typicode.com/posts/" :show-file-list="false" :on-success="handleAvatarSuccess" :on-preview="handlePictureCardPreview" :on-remove="handleRemove">
										<img v-if="imageUrl" :src="imageUrl" class="avatar">
										<i v-else class="el-icon-plus avatar-uploader-icon"></i>
									</el-upload>

								</el-form-item>
								<!-- 多图片上传 -->
								<el-form-item :label="item.label" v-if='item.type=="imgListUpload"'>
									<el-upload action="https://jsonplaceholder.typicode.com/posts/" list-type="picture-card" :on-preview="handlePictureCardPreview" :on-remove="handleRemove">
										<i class="el-icon-plus"></i>
									</el-upload>
									<el-dialog v-model="dialogVisible" size="tiny">
										<img width="100%" :src="dialogImageUrl" alt="">
									</el-dialog>

								</el-form-item>
								<!-- 富文本编辑器 -->
								<el-form-item :label="item.label" v-if='item.type=="editor"'>
									<quill-editor ref="myTextEditor" v-model="content" :config="editorOption"></quill-editor>
								</el-form-item>
								<!-- 提交 -->
								<el-form-item v-if='item.type=="submit"'>
									<el-button type="primary" @click="onSubmit">立即创建</el-button>
									<el-button>取消</el-button>
								</el-form-item>
							</div>
						</el-form>
					</div>
				</el-col>
			</el-row>
			<el-row :gutter="50">
				<el-col :span="8">订单内容</el-col>
			</el-row>
			<el-row :gutter="50">
				<el-table :data="tableData" border style="width: 100%" ref="multipleTable" @selection-change="handleSelectionChange">
					<el-table-column type="selection" width="55"></el-table-column>
					<el-table-column prop="id" label="序号" sortable>
					</el-table-column>
					<el-table-column prop="id" label="商品ID">
					</el-table-column>

					<el-table-column prop="name" label="商品名称">

					</el-table-column>
					<el-table-column prop="buyNumber" label="商品数量">
					</el-table-column>
					<el-table-column prop="price" label="总价格">
					</el-table-column>
					<el-table-column prop="size" label="商品属性">
					</el-table-column>
				</el-table>
			</el-row>
			<!-- <el-form-item label="订单状态"> -->

			<!--  </el-form-item>  -->
			<el-row :gutter="50">
				<el-col :span="8">订单状态</el-col>
			</el-row>
			<el-row :gutter="50">
				<el-col :span="8">
					<el-form>
						<el-form-item label="配送状态">
							<el-switch on-text="配送中" off-text="待配送" v-model="delivery1" width='100' @change='changeState'></el-switch>
						</el-form-item>
						<el-form-item label="确认状态">
							<el-switch on-text="订单确认" off-text="待确认" v-model="delivery2" width='100'></el-switch>
						</el-form-item>
					</el-form>
				</el-col>
			</el-row>
		</div>

	</div>
</template>

<script>
	import { quillEditor } from 'vue-quill-editor';
	export default {
		data() {
			return {
				editorOption: {
					// something config
				},
				activeIndex: '1',
				activeName2: '1',
				form1: [{
						'label': '订单明细',
						"type": 'text',
						"value": '123456'
					}, //输入框。label为标题，type为类型，value为默认值

					{
						'label': '会员id',
						"type": 'text',
						"value": 'aaa'
					},

					{
						'label': '收获电话',
						"type": 'text',
						"value": 'aaa'
					},

					{
						'label': '配送时间段',
						"type": 'text',
						"value": 'aaa'
					},

					{
						'label': '订单备注',
						"type": 'text',
						"value": 'aaa'
					},
				],
				form2: [{
						'label': '订单id',
						"type": 'text',
						"value": 'aaa'
					},
					{
						'label': '收货人',
						"type": 'text',
						"value": 'aaa'
					},
					{
						'label': '收获地址',
						"type": 'text',
						"value": 'aaa'
					},
					{
						'label': '配送方式',
						"type": 'text',
						"value": 'aaa'
					},
				],
				delivery1: false,
				delivery2: false,
				tableData: [{
						"id": "1",
						"name": "蓝晴Fontanesia",
						'buyNumber': '6',
						'price': '268',
						'size': '6-蓝莓口味'
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

				],

			}
		},
		components: {
			quillEditor
		},
		methods: {
			handleSelect(key, keyPath) {
				console.log(key, keyPath);
			},
			handleOpen(key, keyPath) {
				console.log(key, keyPath);
			},
			handleClose(key, keyPath) {
				console.log(key, keyPath);
			},
			handleClick(tab, event) {
				console.log(tab, event);
			},
			onSubmit() {
				console.log('submit!');
			},
			handleAvatarSuccess(res, file) {
				console.log(URL.createObjectURL(file.raw))
				this.imageUrl = URL.createObjectURL(file.raw);
			},
			handleRemove(file, fileList) {
				console.log(file, fileList);
			},
			handlePictureCardPreview(file) {
				this.dialogImageUrl = file.url;
				this.dialogVisible = true;
			},
			changeState() {
				if(this.delivery1) {
					this.$prompt('填写快递单号', '提示', {
						confirmButtonText: '确定',
						cancelButtonText: '取消',
						// inputPattern: /[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/,
						// inputErrorMessage: '邮箱格式不正确'
					}).then(({
						value
					}) => {
						this.$message({
							type: 'success',
							message: '你的快递单号是: ' + value
						});
					}).catch(() => {
						this.delivery1 = false
						this.$message({
							type: 'info',
							message: '取消输入'
						});
					});
				}

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
</style>