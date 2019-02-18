<template>
  <div class="myTableContainer clearfix">
    <el-row :gutter="20">
      <el-col :xs="24" :md="12" :lg="8" class="ant-list-item-content" style="margin-bottom:20px" v-if="addCol">
        <button type="button" class="ant-list-item-content-single" :style="{ height: firstHeight + 'px' }" @click="addList()">
          <span>+</span><span>&nbsp;&nbsp;新增产品</span>
        </button>
      </el-col>
      <el-col :xs="24" :md="12" :lg="8"  v-for="(item,index) in searchResult.data.slice((paginationConfig.currentPage-1)*paginationConfig.pageSize,paginationConfig.currentPage*paginationConfig.pageSize)" :key="index" style="margin-bottom:20px">
        <div class="tableOne" ref="tableOne">
          <div class="ant-card-body" @click="opendetail(index)">
            <div class="ant-card-meta-avatar">
              <img :src="item.imgUrl" alt="">
            </div>
            <div class="ant-card-meta-detail">
              <div class="ant-card-meta-title">
                <a>{{item.id}}、{{item.name}}</a>
              </div>
              <div class="ant-card-meta-description">
                <p v-for="(value, key, indexs) in searchResult.midContent" :key="indexs">{{value}}：{{item[key]}}</p>
              </div>
              <div class="ant-card-meta-service" @click.stop="openService(index)">+</div>
            </div>
          </div>
          <ul class="ant-card-actions clearfix">
            <li @click="stateChange(index)">
              <span v-text="item.state?'停用':'启用'"></span>
            </li>
            <li>
              <el-dropdown @command="handleCommand" trigger="click" placement="bottom">
                <span class="el-dropdown-link">
                  操作<i class="el-icon-arrow-down el-icon--right"></i>
                </span>
                <el-dropdown-menu slot="dropdown">
                  <el-dropdown-item :command="{type:value.type,params:index}" v-for="(value, key, indexs) in operateConfig" :key="indexs">{{value.name}}</el-dropdown-item>
                </el-dropdown-menu>
              </el-dropdown>
            </li>
          </ul>
        </div>
      </el-col>
    </el-row>
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="paginationConfig.currentPage"
        :page-sizes="paginationConfig.pageSizes"
        :page-size="paginationConfig.pageSize"
        layout= 'total, sizes, prev, pager, next, jumper'
        :total="searchResult.data.length">
      </el-pagination>
  </div>
</template>
<script>
export default {
  props: {
    type: {
      type: String,
      default: "CN"
    },
    tableConfig:Object
  },
  data() {
    return {
      addCol:true,
      firstHeight:'',
      searchResult:this.tableConfig,
      paginationConfig:this.tableConfig.paginationConfig,
      operateConfig:this.tableConfig.operateConfig
    };
  },
  mounted: function(){
    this.firstHeight = this.$refs.tableOne[0].offsetHeight;
  },
  methods: {
    onSubmit() {
      console.log("submit!");
    },
    stateChange(index){
      this.$emit('tableFunction', 'stateChange',index)
    },
    addList(){
      this.$emit('tableFunction', 'addList')
    },
    openService(index){
      this.$emit('tableFunction', 'openService',index)
    },
    opendetail(index){
      this.$emit('tableFunction', 'check',index)
    },
    handleCommand(command){
        this.$emit('tableFunction', command.type ,command.params)
    },
    parentHandleclick() {
        console.log(this.tableConfig);
        this.searchResult=this.tableConfig;
    },
    handleSizeChange(size){
        this.paginationConfig.pagesize = size;
    },
    handleCurrentChange: function(currentPage){
        this.paginationConfig.currentPage = currentPage;
        console.log(currentPage)
        if(currentPage!='1'){
          this.addCol = false;
        }else{
          this.addCol = true;
        }
    },
  }
};
</script>
<style rel="stylesheet/scss" lang="scss" scoped>
*{
  margin: 0;
  padding: 0
}
.el-button{
    padding: 12px 20px;
}
ul li{
list-style-type:none;
}
.clearfix:after {clear:both;font-size:0; height:0; display:block; visibility:hidden; content:" "; line-height:0}
.myTableContainer{
  background: #f0f2f5;
  padding: 24px 50px;
  color: rgba(0,0,0,.65);
  font-size: 14px;
  .ant-list-item-content{
      .ant-list-item-content-single{
        line-height: 1.499;
        display: inline-block;
        font-weight: 400;
        text-align: center;
        touch-action: manipulation;
        cursor: pointer;
        background-image: none;
        white-space: nowrap;
        padding: 0 15px;
        font-size: 14px;
        border-radius: 4px;
        height: 32px;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
        transition: all .3s cubic-bezier(.645,.045,.355,1);
        position: relative;
        box-shadow: 0 2px 0 rgba(0,0,0,.015);
        color: rgba(0,0,0,.65);
        background-color: #fff;
        border: 1px solid #d9d9d9;
        background-color: #fff;
        border-color: #d9d9d9;
        border-radius: 2px;
        color: rgba(0,0,0,.45);
        width: 100%;
        border-style: dashed;
        &:hover{
          color: #40a9ff;
          background-color: #fff;
          border-color: #40a9ff;
        }
      }
  }
  .tableOne{
    font-size: 14px;
    font-variant: tabular-nums;
    line-height: 1.5;
    color: rgba(0,0,0,.65);
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    list-style: none;
    background: #fff;
    border-radius: 2px;
    position: relative;
    transition: all .3s;
    width: 100%;
    height: auto;
    order: 1px solid #e8e8e8;
    cursor: pointer;
    &:hover{
      box-shadow: 0 2px 8px rgba(0,0,0,.09);
      border-color: rgba(0,0,0,.09);
    }
    .ant-card-body{
        padding: 24px;
        zoom: 1;
        .ant-card-meta-avatar{
          width: 48px;
          height: 48px;
          margin-right: 16px;
          float: left;
          img{
            width:100%;
            height:100%;
            border-radius: 100%;
          }
        }
        .ant-card-meta-detail{
            overflow: hidden;
            .ant-card-meta-title{
              margin-bottom: 12px;
              font-size: 16px;
              text-overflow: ellipsis;
              white-space: nowrap;
              color: rgba(0,0,0,.85);
              font-weight: 500;
              a{
                  color: rgba(0,0,0,.85);
                  display: inline-block;
                  max-width: 100%;
              }
            }
            .ant-card-meta-description{
              color: rgba(0,0,0,.45);
            }
            .ant-card-meta-service{
              position: absolute;
              top: 24px;
              right: 24px;
              font-size: 40px;
              text-align: center;
              line-height: 20px;
            }
        }
    }
    .ant-card-actions{
      background: #f7f9fa;
      border-top: 1px solid #e8e8e8;
      li{
        width: 50%;
        float: left;
        text-align: center;
        margin: 12px 0;
        color: rgba(0,0,0,.45)!important;
        .el-dropdown-link{
          color: rgba(0,0,0,.45)!important;
          padding: 10px 20px
        }
      }
      li:not(:last-child){
        border-right: 1px solid #e8e8e8;
      }
    }
  }
}
</style>