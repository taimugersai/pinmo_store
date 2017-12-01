<template>
    <div class="header">
        <el-row>
            <div class="logo"><span v-if='active==1'>品默</span><span v-if='active==2'>麻麻香</span>后台管理系统
            </div>
            <div class="logo info"> <span>店名:{{name}}</span><span>联系方式:{{phone}}</span><span>地址:{{address}}</span></div>
            <div class="user-info">
                <el-dropdown trigger="click" @command="handleCommand">
                    <span class="el-dropdown-link">
                        <img class="user-logo" :src="imgUrl">
                        {{name}}
                        <i class="el-icon-caret-bottom el-icon--right"></i>
                    </span>
                    <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item command="loginout">退出</el-dropdown-item>
                    </el-dropdown-menu>
                </el-dropdown>
            </div>
        </el-row>
        <el-row>
            <el-menu theme="dark" background-color="#242f42"
              text-color="#fff"
              active-text-color="#20a0e1"
              :default-active="onRoutes" class="el-menu-demo" mode="horizontal" @select="handleSelect" >
              
            <router-link :to="{ path: 'order'}" ><el-menu-item index='order'>订单管理</el-menu-item></router-link>
            <router-link :to="{ path: 'bigOrder'}" ><el-menu-item index='bigOrder'>兑换管理</el-menu-item></router-link>
              
            </el-menu>

        </el-row>
        
        
    </div>

</template>
<script>

    import axios from 'axios';
    import api from '../../api/api.json';
    export default {
        data() {
            return {
                name: '',
                phone:'',
                address:'',
                active:1,
                imgUrl:'../../../static/img/pinmo_logo.png'
            }
        },
        mounted:function(){
            this.getStoreInfo();
            if(!localStorage.getItem('type')){
                localStorage.setItem('type',1);
            }
            this.active=localStorage.getItem('type');
            
        },
        computed:{
            username(){
                var username=""
                if(this.active==1){
                    username="品默"
                    this.imgUrl="../../../static/img/pinmo_logo.png"
                }else if(this.active==2){
                    username="麻麻香"
                    this.imgUrl="../../../static/img/mama_logo.png"
                }
                //let username = localStorage.getItem('ms_username');
                return username;
            },
            onRoutes(){
                //console.log(this.$route.path.replace('/',''))
                return this.$route.path.replace('/','');
            }
        },
        methods:{
            getStoreInfo(){
                let self = this;
                //alert(api.baseUrl +'/store_api/store')
                axios.get(api.baseUrl +'/profile',
                ).then((res) => {
                    if(res.data.responseCode == 0) {
                        self.$message({
                          type: 'info',
                          message: `网络异常，获取失败`
                        });
                    } else {
                        //console.log(res)
                        self.name=res.data.display_name
                        self.phone = res.data.tel
                        self.address = res.data.address
                        self.active = res.data.brand_id
                    }
                }).catch(function(error) {
                    //console.log(error);
                });
            },
            handleCommand(command) {
                var self=this;
                if(command == 'loginout'){

                    localStorage.removeItem('token')
                    axios.defaults.headers.common['Authorization'] = 'Bearer ';
                    this.$router.push('/login');
                }else{
                    self.active=command
                    localStorage.setItem('type',command);
                     this.$router.go(0)
                }
            },
           
        }
    }
</script>
<style scoped>
    .header {
        position: relative;
        box-sizing: border-box;
        width: 100%;
        font-size: 22px;
        line-height: 70px;
        color: #fff;
    }
    .header .logo{
        float: left;
        width:250px;
        text-align: center;
    }
    .user-info {
        float: right;
        padding-right: 50px;
        font-size: 16px;
        color: #fff;
    }
    .user-info .el-dropdown-link{
        position: relative;
        display: inline-block;
        padding-left: 50px;
        color: #fff;
        cursor: pointer;
        vertical-align: middle;
        font-size: 16px;
    }
    .user-info .user-logo{
        position: absolute;
        left:0;
        top:15px;
        width:40px;
        height:40px;
        border-radius: 50%;
    }
    .el-dropdown-menu__item{
        text-align: center;
    }
    .info{
        font-size: 20px;
        width: auto!important;
    }
    .info span{
        margin-left: 20px;
    }
    .el-menu-demo{
        width: 100%;
    }
    .clear:after{
        clear:both;
        display: block;
    }
</style>
