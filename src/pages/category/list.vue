<template>
    <div>
        <!-- 按钮开始 -->
        <el-button size="small" type="info" @click="toAddHandler">添加</el-button>
        <el-button size="small" type="danger">修改</el-button>
        <!-- 按钮结束 -->
  
        <!-- 表格开始 -->
        <el-table :data="category">
            <el-table-column prop="id" label="编号"></el-table-column>
            <el-table-column prop="name" label="名称"></el-table-column>
            <el-table-column prop="num" label="数量"></el-table-column>
            <el-table-column prop="icom" label="图片"></el-table-column>
            <el-table-column prop="parentId" label="父名"></el-table-column>
           <el-table-column>
               <template v-slot="slot">
                <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                <a href="" @click.prevent="toUpdateHandler(slot.row)">修改</a>
               </template>
           </el-table-column>
        </el-table>
         <!-- 表格结束 -->
         <!-- 分页开始 -->
         <el-pagination layout="prev, pager, next"
          :total="50">>
         </el-pagination>
         <!-- 分页结束 -->
         <!-- 模态框开始 -->
         <el-dialog
       :title="title"
       :visible.sync="visible"
       width="60%">
       {{form}}
       <el-form model="form" label-width="80px">
           <el-form-item label="编号"><el-input v-model="form.id"/>    </el-form-item>
           <el-form-item label="名称"><el-input v-model="form.name"/>    </el-form-item>

       </el-form>
       <span slot="footer" class="dialog-footer">
      <el-button @click="closeModalHandler">取 消</el-button>
      <el-button type="primary" @click="submitModalHandler">确 定</el-button>
      </span>
        </el-dialog>
         <!-- 模态框结束 -->
    </div>
</template>
<script>
import request from '@/utils/request'
import querystring from 'querystring'
export default {
    data(){
        return{
           title:"title",
           visible:false,
           category:[],
           form:{}
        }

    },

    created(){
        this.dataLoad();
    },

    methods:{
        submitModalHandler(){
            let url='http://localhost:6677/category/saveOrUpdate';
            request({
                url,
                method:"POST",
                Headers:{
                    "Content-Type":"application/x-www-form-urlencoder"
                },
                data:querystring.stringify(this,form)
            }).then((response)=>{
             this.closeModalHandler(),
             this.dataLoad(),
             this.$message({
                 type:"success",message:response.message
             })

            })
        },
        dataLoad(){
            let url='http://localhost/6677/category/findAll'
            request.get(url).then((response)=>{
                this.category=response.data;
            })
        },
        closeModalHandler(){
            this.visible=false;
        },
        toAddHandler(){
        this.title="录入员工信息",
        this.visible=true;
        },
        toDeleteHandler(id){
       this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          });
        })
        },
        toUpdateHandler(row){
            this.form=row;
//模态框显示当前行信息

            this.title="更新类别信息"
            this.visible=true;
        }

    }
    
}
</script>
<style scoped>

</style>