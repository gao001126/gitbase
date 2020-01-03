<template>
    <div>
        <!-- 按钮开始 -->
        <el-button size="small" type="success" @click="toAddhandler">添加</el-button>
        <el-button size="small" type="danger" >批量删除</el-button>
        <!-- 按钮结束 -->
        <!-- 表格开始 -->
        <el-table :data="categorys" height="250" border style="width: 100%">
            <el-table-column type="selection"></el-table-column>
        <el-table-column prop="id" label="编号" width="180"></el-table-column>
        <el-table-column prop="name" label="栏目名称" width="180"></el-table-column>
        <el-table-column prop="number" label="序号"></el-table-column>
        <el-table-column prop="parentId" label="父栏目"></el-table-column>
        <el-table-column  label="操作">
            <template v-slot="slot">
                 <a href="" @click.prevent="toUpdateHandler" class="el-icon-edit"></a>
                 <a href="" @click.prevent="toDeleteHandler" class="el-icon-delete"></a>
                 <a href="" >详情</a>
            </template>
        </el-table-column>
        </el-table>
        <!-- 表格结束 -->
        <!-- 模态框开始 -->
        <el-dialog
            :title="title"
            :visible.sync="visible"
             width="40%" >
 ------------ {{form}}
    <el-form  :model="form" label-width="80px">
        <el-form-item  label="栏目名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item> 
    </el-form> 
    <el-form :model="form"  label-width="80px">
        <el-form-item  label="序号">
          <el-input v-model="form.num"></el-input>
        </el-form-item> 
    </el-form> 
    
      <span slot="footer" class="dialog-footer">
    <el-button @click="closeModalHandler">取 消</el-button>
    <el-button type="primary" @click="submitHandler">确 定</el-button>
  </span>
</el-dialog>
        <!-- 模态框结束 -->
    </div>
</template>

<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    methods:{
        loadData(){
             //vue实例创建完毕
        let url="http://localhost:6677/category/findAll"
     request.get(url).then((response)=>{
         this.categorys=response.data
     })
        },
        toAddhandler(){
            this.visible=true
            this.title="添加栏目信息"
        },
        toUpdateHandler(){
            this.visible=true
            this.title="修改栏目信息"
        },
        toDeleteHandler(){
            this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      
        },
        closeModalHandler(){
            this.visible=false

        },
        submitHandler(){
             let url="http://localhost:6677/category/saveOrUpdate"
          request({
            url,
            method:"POST",
            headers:{
              "Content-Type":"application/x-www-form-urlencoded"
            },
            data:querystring.stringify(this.form)
          }).then((response)=>{
            //请求结束
            this.closeModalHandler();
            this.loadData()
            this.$message({
              type:"success",
              message:response.message
        
            })
          })}
    },
    data(){
        return{
       visible:false,
       categorys:[],
       form:{
           type:"category"
       }
    //    form什么类型都能接受
        }
    },
    created(){
       this.loadData();
    }
}
</script>

<style lang="stylus" scoped>

</style>