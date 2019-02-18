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
      <el-dialog title="应用信息" :visible.sync="dialogFormVisible" @close="clickForm('detailsForm')">
        <el-form :model="detailsForm" ref="detailsForm" label-width="100px" :rules="rules">
          <el-form-item label="地址">
            <el-input :disabled="ifNoEdit" v-model="detailsForm.address"></el-input>
          </el-form-item>
          <el-form-item label="识别码" prop="idenCode">
            <el-input :disabled='ifNoNewAdd' v-model="detailsForm.idenCode"></el-input>
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
          <el-button @click="clickForm('detailsForm')">关闭</el-button>
          <el-button type="primary" @click="editSubmlit('detailsForm')" v-show="!ifNoEdit">确定</el-button>
        </div>
      </el-dialog>
      <el-dialog title="应用服务" :visible.sync="dialogServiceVisible">
        <el-table :data="serviceListDialog" style="width: 100%">
          <el-table-column label="序号" width="180">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.id }}</span>
            </template>
          </el-table-column>
          <el-table-column label="姓名" width="180">
            <template slot-scope="scope">
              <span style="margin-left: 10px">{{ scope.row.name }}</span>
            </template>
          </el-table-column>
          <el-table-column label="操作">
            <template slot-scope="scope">
              <el-button size="mini" @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
              <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
        <div slot="footer" class="dialog-footer">
          <el-button @click="dialogServiceVisible = false">关闭</el-button>
          <!-- <el-button type="primary" @click="editSubmlit('detailsForm')" v-show="!ifNoEdit">确定</el-button> -->
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
        dialogServiceVisible: false,
        ifNoEdit:false,
        ifNoNewAdd:true,
        operateIndex:0,
        serviceListDialog:[],
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
            {id:"1",code:"com-ild-osi-lbs",
            name:"物联网地理位置服务2",
            address:"http://lbs.nbeport.com",
            state:false,upTime:"2019-01-14 16:42:19",
            imgUrl:require("@/assets/img/payIcon.png"),
            idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',
            aes:'密钥1',
            exp:'详细说明',
            serviceList:[
              {id:"1",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"2",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"3",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"4",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            },
            {id:"2",code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明',
            serviceList:[
              {id:"2",versionCode:'1.1',code:"com-ild-osi-lbs",name:"物联网地理位置服务2",address:"http://lbs.nbeport.com:80",state:false,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
              {id:"3",versionCode:'1.2',code:"com-ild-osi-lbs",name:"物联网地理位置服务3",address:"http://lbs.nbeport.com:80",state:true,upTime:"2019-01-14 16:42:19",imgUrl:require("@/assets/img/payIcon.png"),idenCode:'459d21d80a19dc6e9af371f64fe6bdbaebcbe1bf',aes:'密钥2',exp:'详细说明'},
            ]
            }
          ],
          midContent:{code:"代码",address:"地址",upTime:"更新时间"},
          paginationConfig:{
            pageSizes:[11, 23, 35],
            pageSize:11,
            currentPage:1,//初始页
            layout:'total, sizes, prev, pager, next, jumper'
          },
          operateConfig:[
            {type:'check',name:'查看'},
            {type:'edit',name:'编辑'},
            {type:'secretKey',name:'密钥'},
            {type:'menu',name:'菜单'},
            {type:'interface',name:'接口'},
            {type:'del',name:'删除'}
          ]
        }
      };
    },
    // computed:{
    //   serviceListDialog: function () {
    //   return this.myTable.data[0].serviceList;
    // },
    // },
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
      clickForm(formName){
        for(var key in this.detailsForm){
            this.detailsForm[key]='';
        }
        this.dialogFormVisible = false;
        this.ifNoEdit = false;
        this.ifNoNewAdd = true;
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
      handleEdit(index, row) {
        console.log(index, row);
      },
      handleDelete(index, row) {
        console.log(index, row);
      },
      tableFunction(switchIndex,index){
        this.operateIndex = index;
        switch (switchIndex) {
          case 'stateChange':
            this.myTable.data[index].state = !this.myTable.data[index].state;
            break;
          case 'addList':
            this.dialogFormVisible = true;
            this.ifNoNewAdd = false;
            break;
          case 'check':
            this.dialogFormVisible = true;
            this.ifNoEdit = true;
            this.deepCopy(this.detailsForm,this.myTable.data[index]);
            break;
          case 'edit':
            this.dialogFormVisible = true;
            this.ifNoEdit = false;
            console.log(this.detailsForm);
            this.deepCopy(this.detailsForm,this.myTable.data[index]);
            break;
          case 'del':
            this.delOpen();
            break;
          case 'openService':
            this.dialogServiceVisible = true;
            this.serviceListDialog = this.myTable.data[index].serviceList;
            break;
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