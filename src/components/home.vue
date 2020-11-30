<template>
  <div>
    <el-button type="primary" @click="logout()">退出</el-button>
  </div>
</template>

<script>
  export default {
    methods: {
      logout() {
        window.localStorage.clear()
        this.$message.success("退出成功！")
        this.$router.push("/login")
      }
    }
  }
</script>

<style lang="less" scoped>
</style>
