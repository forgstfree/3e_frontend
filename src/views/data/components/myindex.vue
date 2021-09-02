<template>
  <div class="my-upload-container">
    <el-upload
      ref="xlsupload"
      class="upload-demo"
      :action="uploadFileUrl"
      drag
      accept=".csv, .xls, .xlsx"
      :data="fileform"
      :on-preview="handlePreview"
      :on-remove="handleRemove"
      :file-list="fileList"
      :auto-upload="false"
      :before-upload="beforeUpload"
    >
      <i class="el-icon-upload" />
      <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
      <!-- <el-button slot="trigger" size="small" type="primary">选取文件</el-button> -->
      <div slot="tip" class="el-upload__tip">只能上传结构化文件，且不超过xkb</div>
    </el-upload>
    <el-button style="margin-left: 10px" size="small" type="success" @click="submitUpload"
      >上传到服务器</el-button
    >
  </div>
</template>
<script>
export default {
  data() {
    return {
      fileList: [],
      uploadFileUrl: process.env.VUE_APP_BASE_URL + "/file/structured/",
      fileform: {
        filename: "",
        uploader: "admin",
      },
    };
  },
  methods: {
    beforeUpload(file) {
      console.log(file);
      this.fileform.filename = file.name;
    },
    submitUpload() {
      this.$refs.xlsupload.submit();
    },
    handleRemove(file, fileList) {
      console.log(file, fileList);
    },
    handlePreview(file) {
      console.log(file);
    },
  },
};
</script>
<style>
.my-upload-container {
  text-align: center; /*让div内部文字居中*/
}
</style>
