<template>
  <div class="">
    <el-button :loading="downloadLoading" style="margin:0 0 20px 20px;" type="primary" icon="el-icon-download" @click="dialogTableVisible = true">
      上传
    </el-button>
    <el-dialog v-el-drag-dialog :visible.sync="dialogTableVisible" title="Shipping address" @dragDialog="handleDrag" />
  </div>
</template>

<script>
import elDragDialog from '@/directive/el-drag-dialog' // base on element-ui

export default {
  name: 'UploadFile',
  directives: { elDragDialog },
  data() {
    return {
      dialogTableVisible: false
    }
  },
  methods: {
    // v-el-drag-dialog onDrag callback function
    handleDrag() {
      this.$refs.select.blur()
    }
  }
}
</script>
