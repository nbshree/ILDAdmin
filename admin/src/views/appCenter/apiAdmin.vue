<template>
  <el-container>
    <el-main style="padding:0">
      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="ruleForm clearfix">
          <el-col :span="4">
            <el-form-item label="代码:" prop='code'>
              <el-input v-model="ruleForm.code"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="4">
            <el-form-item label="名称:" prop='name'>
              <el-input v-model="ruleForm.name"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="4">
            <el-form-item label="地址:" prop='address'>
              <el-input v-model="ruleForm.address"></el-input>
            </el-form-item>
          </el-col>
          <el-col :span="8">
            <el-form-item label="活动时间">
              <el-col :span="11">
                <el-form-item prop="date1">
                  <el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.date1" style="width: 100%;"></el-date-picker>
                </el-form-item>
              </el-col>
              <el-col class="line" :span="2" style="text-align: center">-</el-col>
              <el-col :span="11">
                <el-form-item prop="date2">
                   <el-date-picker type="date" placeholder="选择日期" v-model="ruleForm.date2" style="width: 100%;"></el-date-picker>
                </el-form-item>
              </el-col>
            </el-form-item>
          </el-col>
          <el-col :span='4' align="right" style="padding-right:50px">
            <el-form-item>
              <el-button type="primary" @click="submitForm('ruleForm')">查询</el-button>
              <!-- <el-button @click="resetForm('ruleForm')">新增</el-button> -->
            </el-form-item>
          </el-col>
      </el-form>
      <my-table v-bind:tableConfig="myTable" @tableFunction="tableFunction" ref="mychild"></my-table>
      <el-dialog title="应用信息" :visible.sync="dialogFormVisible">
        <el-form :model="detailsForm" ref="detailsForm" label-width="100px" :rules="rules">
          <el-form-item label="地址">
            <el-input :disabled="ifNoEdit" v-model="detailsForm.address"></el-input>
          </el-form-item>
          <el-form-item label="识别码" prop="idenCode">
            <el-input :disabled='true' v-model="detailsForm.idenCode"></el-input>
          </el-form-item>
          <el-form-item label-width="0">
            <el-col :span="12">
              <el-form-item label="代码:" prop="code">
                <el-input :disabled="ifNoEdit" v-model="detailsForm.code"></el-input>
              </el-form-item>
            </el-col>
            <el-col :span="12">
              <el-form-item label="名称:" prop="name">
                <el-input :disabled="ifNoEdit" v-model="detailsForm.name"></el-input>
              </el-form-item>
            </el-col>
          </el-form-item>
          <el-form-item label="AES密钥">
            <el-input :disabled="ifNoEdit" v-model="detailsForm.aes"></el-input>
          </el-form-item>
          <el-form-item label="详细说明">
            <el-input :disabled="ifNoEdit" type="textarea" resize='none' v-model="detailsForm.exp"></el-input>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogFormVisible = false">关闭</el-button>
          <el-button type="primary" @click="editSubmlit('detailsForm')" v-show="!ifNoEdit">确定</el-button>
        </div>
      </el-dialog>
    </el-main>
  </el-container>
</template>
<script>
import myTable from './components/myTable'
  export default {
    components: { myTable },
    data() {
      return {
        dialogFormVisible: false,
        ifNoEdit:false,
        operateIndex:'',
        ruleForm: {
          name: '',
          address:'',
          code:''
        },
        detailsForm:{
          address:'',
          code:'',
          idenCode:'',
          name:'',
          aes:'',
          exp:''
        },
        rules: {
          name: [
            { required: true, message: '请填写名称', trigger: 'change' }
          ],
          idenCode: [
            { required: true, message: '请填写识别码', trigger: 'change' }
          ],
          code: [
            { required: true, message: '请填写代码', trigger: 'change' }
          ],
          address: [
            { required: true, message: '请填写地址', trigger: 'change' }
          ],
        },
        myTable:{
          data:[
            {id:"1",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥1',exp:'详细说明'},
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"3",code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            {id:"4",code:"com-ild-osi-lbs",name:"物联网地理位置服务4",address:"http://lbs.nbeport.com",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'}
          ],
          midContent:{code:"代码",address:"地址",upTime:"更新时间"},
          paginationConfig:{
            pageSizes:[11, 23, 35],
            pageSize:11,
            currentPage:1,//初始页
            layout:'total, sizes, prev, pager, next, jumper'
          }
        }
      };
    },
    methods: {
      deepCopy(oldObj,obj){
        for(var vl in obj){
            if(typeof obj[vl] === 'object' && obj[vl] !== null){
                oldObj[vl] = this.deepCopy(obj[vl]);
            }else{
                oldObj[vl] = obj[vl];
            }       
        }
      },
      submitForm(formName) {
        console.log(this.detailsForm)
        this.$refs[formName].validate((valid) => {
          if (valid) {
            alert('submit!');
            this.$refs[formName].resetFields();
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      resetForm(formName) {
        this.$refs[formName].resetFields();
      },
      editSubmlit(formName){
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.deepCopy(this.myTable.data[this.operateIndex],this.detailsForm);
            this.$refs.mychild.parentHandleclick();
            this.dialogFormVisible = false;
          } else {
            console.log('error submit!!');
            return false;
          }
        });
      },
      delOpen() {
        this.$confirm('此操作将永久删除该应用, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      },
      tableFunction(switchIndex,index){
        this.operateIndex = index;
        var indexs = this.operateIndex ;
        switch (switchIndex) {
          case 'stateChange':
            this.myTable.data[indexs].state = !this.myTable.data[indexs].state;
            break;
          case 'check':
              this.dialogFormVisible = true;
              this.ifNoEdit = true;
              this.deepCopy(this.detailsForm,this.myTable.data[indexs]);
              break;
          case 'edit':
              this.dialogFormVisible = true;
              this.ifNoEdit = false;
              console.log(this.detailsForm);
              this.deepCopy(this.detailsForm,this.myTable.data[indexs]);
              break;
          case 'del':
              this.delOpen();
          default:
            break;
        }
      }
    }
  }
</script>
<style rel="stylesheet/scss" lang="scss" scoped>
.clearfix:after {clear:both;font-size:0; height:0; display:block; visibility:hidden; content:" "; line-height:0}
.el-container{
  margin-top: 50px;
  .ruleForm{
    padding-bottom: 30px
  }
}
</style>