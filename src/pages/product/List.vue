<template>
    <div>
        <!-- 按钮开始 -->
        <el-button size="small" type="success" @click="toAddhandler">添加</el-button>
        <el-button size="small" type="danger" >批量删除</el-button>
        <!-- 按钮结束 -->
        <!-- 表格开始 -->
        <el-table :data="products" height="250" border style="width: 100%">
        <el-table-column prop="id" label="编号" width="180"></el-table-column>
        <el-table-column prop="name" label="产品名称" width="180"></el-table-column>
        <el-table-column prop="description" label="描述"></el-table-column>
        <el-table-column prop="price" label="价格"></el-table-column>
        <el-table-column prop="status" label="所属产品"></el-table-column>
        <el-table-column label="操作">
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
        <el-form-item  label="名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item> 
    </el-form> 
    <el-form :model="form"  label-width="80px">
        <el-form-item  label="价格">
          <el-input v-model="form.price"></el-input>
        </el-form-item> 
    </el-form> 
    <el-form  label-width="80px">
         <el-form-item label="所属栏目" :label-width="formLabelWidth">
      <el-select  placeholder="请选择">
        <el-option label="栏目一" ></el-option>
        <el-option label="栏目二" ></el-option>
      </el-select>
    </el-form-item>
    </el-form> 
    <el-form  label-width="80px"><el-form-item label="介绍">
          <el-input ></el-input>
        </el-form-item> 
    </el-form> 
    <el-form  label-width="80px"><el-form-item label="产品主图">
        
              <el-button type="primary">点击上传</el-button>
        
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
        let url="http://localhost:6677/product/findAll"
        request.get(url).then((response)=>{
        this.products=response.data
     })
        },
        toAddhandler(){
            this.visible=true
        },
        toUpdateHandler(){
            this.visible=true
        },
        toDeleteHandler(){
            this.visible=true
        },
        closeModalHandler(){
            this.visible=false

        },
         submitHandler(){
             let url="http://localhost:6677/product/saveOrUpdate"
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
       products:[],
       form:{
           type:"product"
       }

        }
    },
    created(){
          this.loadData();
    }
}
</script>

<style lang="stylus" scoped>

</style>