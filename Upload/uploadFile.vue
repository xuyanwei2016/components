<template>
  <!--上传文件-->
  <div class="uploadFile">
    <div class="pdfCss" v-if="fileData.vueStatus">{{imgForm.relatedFile?imgForm.relatedFileName:'暂无文件'}}<span
      v-if="fileList.length>0" @click="downFile(imgForm.relatedFile)" style="cursor: pointer;color: #4285F4">下载</span>
    </div>
    <el-upload class="upload-demo uploadFileCss" :action="fileData.action" :on-exceed="handleFileExceed"
               :before-upload="beforeFileUpload" :limit="fileData.limit" :on-success="handleFileSuccess" multiple
               :on-remove="handleFileRemove" :file-list="fileList" v-if="!fileData.vueStatus">
      <el-button :loading="uploading" type="primary">选择文件</el-button>
      <span v-if="fileList.length>0" @click="downFile(fileData.relatedFile)"
            style="cursor: pointer;color: #4285F4">下载</span>
      <!--<span>{{resourceForm.pdf_zh}}</span>-->
    </el-upload>
    <div class="el-upload__tip" v-if="!fileData.vueStatus" style="color:#ccc">可上传文件类型包括Word、PDF，大小不超过2M</div>
  </div>
</template>

<script>
  export default {
    name: 'HelloWorld',
    props: ['fileData'],
    data() {
      return {
        uploading: false,
        file: {
          relatedFile: '',
          relatedFileName: '',
        },
        fileList: [],
      }
    },
    created() {
    },
    watch: {},
    mounted() {
      if (this.fileData.relatedFile) {
        let file = {
          name: this.fileData.relatedFileName,
          url: this.fileData.uploadUrl + this.fileData.relatedFile,
        };
        this.fileList = [file];
        this.file.relatedFile=this.file.relatedFile;
        this.file.relatedFileName=this.file.relatedFileName;
      }
    },
    methods: {
      handleFileExceed(files, fileList) {
        this.$message.warning(`单次上传最多${this.fileData.limit}个文件`);
      },
      beforeFileUpload(file) {
        const isPDF = file.type === 'application/pdf' || file.type == 'application/vnd.openxmlformats-officedocument.wordprocessingml.document' || file.type == 'application/msword';
        const isLt10M = file.size / 1024 / 1024 < 2;
        if (!isPDF) {
          this.$message.error('附件文件只能是 PDF、WORD 格式!')
          return isPDF;
        }
        if (!isLt10M) {
          this.$message.error('上传文件大小不能超过 2MB!');
          return isLt10M
        }
        this.uploading = true
      },
      handleFileSuccess(response, file, fileList) {
        console.log('上傳成功', response, file)
        this.file.relatedFile = response;
        this.file.relatedFileName = file.name;
        this.uploading = false;
        this.$emit('uploadFile',this.file);
      },
      handleFileRemove(response, file, fileList) {//删除pdf
        this.file.relatedFile = null;
        this.file.relatedFileName = null;
        this.fileList = [];
        this.$emit('uploadFile',this.file);
      },

      /*下载*/
      downFile(url) {
        let openUrl = `${process.env.BASE_API}/file/file/download/${url}`;
        window.open(openUrl);
      },
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
