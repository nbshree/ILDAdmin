<template>
  <el-container>
    <el-aside>
        <avatar-upload></avatar-upload>
    </el-aside>
    <el-main>
        <personal-center></personal-center>
    </el-main>
  </el-container>
</template>

<script>
import personalCenter from './components/personalCenter'
import avatarUpload from './components/avatarUpload'

export default {
  components: { personalCenter , avatarUpload }
}
</script>

<style scoped>
.el-container{
  margin-top: 50px;
}
</style>
