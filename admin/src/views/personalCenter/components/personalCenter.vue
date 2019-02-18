<template>
  <el-form ref="form" :model="form" label-width="80px">
    <el-form-item label-width=0>
      <el-col :span="6">
        <el-form-item label="用户名：">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
      </el-col>
    </el-form-item>
    <el-form-item label-width=0>
      <el-col :span="6">
        <el-form-item label="用户类型:">
          <el-select v-model="form.region" placeholder="请选择用户类型" style="width:100%">
            <el-option label="用户类型一" value="shanghai"></el-option>
            <el-option label="用户类型二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
      </el-col>
    </el-form-item>
    <el-form-item label-width=0>
      <el-col :span="6">
        <el-form-item label="用户实名:">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
      </el-col>
      <el-col :span="6"  :offset="6">
        <el-form-item label="性别:">
          <el-select v-model="form.region" placeholder="请选择性别" style="width:100%">
            <el-option label="男" value="shanghai"></el-option>
            <el-option label="女" value="beijing"></el-option>
          </el-select>
        </el-form-item>
      </el-col>
    </el-form-item>
    <el-form-item label-width=0>
      <el-col :span="6">
        <el-form-item label="电子邮件:">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
      </el-col>
      <el-col :span="6"  :offset="6">
        <el-form-item label="手机号码:">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
      </el-col>
    </el-form-item>
    <el-form-item label-width=0>
      <el-col :span="18">
        <el-form-item label="用户角色:">
          <el-select v-model="form.region" placeholder="请选择用户角色" style="width:100%">
            <el-option label="用户角色一" value="shanghai"></el-option>
            <el-option label="用户角色二" value="beijing"></el-option>
          </el-select>
        </el-form-item>
      </el-col>
    </el-form-item>
    <el-form-item>
      <el-button type="primary" @click="onSubmit">删除</el-button>
      <el-button>修改</el-button>
    </el-form-item>
    <el-form-item label-width=0>
      <el-col :span="18">
        <label>当前的个人资料完整程度：</label>
        <el-progress :percentage="80" color="#8e71c7"></el-progress>
      </el-col>
    </el-form-item>
  </el-form>
</template>
<script>
export default {
  props: {
    type: {
      type: String,
      default: "CN"
    }
  },
  data() {
    return {
      form: {
        name: "",
        region: "",
        date1: "",
        date2: "",
        delivery: false,
        type: [],
        resource: "",
        desc: ""
      }
    };
  },
  methods: {
    onSubmit() {
      console.log("submit!");
    }
  }
};
</script>