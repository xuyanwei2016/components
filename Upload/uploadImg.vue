<template>
  <!--单个图片上传-->
  <div class="uploadImg1">
    <div  v-if="!img.relatedFile&&imgData.vueStatus">暂无文件</div>
    <el-upload class="avatar-uploader"
               :action="imgData.action" :show-file-list="false" :on-remove="handleRemove" :on-success="handlePicSuccess" :before-upload="beforePicUpload">

      <img :src="`${imgData.uploadUrl}?fileName=${img.relatedFile}&isOnLine=true`" style="width: 178px;height:178px;object:fit;" v-if="img.relatedFile">
      <div class="addImg" v-if="!img.relatedFile&&!imgData.vueStatus">
        <img src="./addImg.png">
      </div>
      <div class="deleteImg" @click.stop="deleteIMG" v-if="img.relatedFile">
        <i class="el-icon-close"></i>
      </div>
    </el-upload>
    <span class="tip" style="margin-left: 0; color: #ccc; height: 30px;line-height: 30px;font-size: 12px">仅支持jpg、jpeg、png、gif格式，不超过2M</span>
  </div>
</template>

<script>
  import './index.css';
  export default {
    name: 'HelloWorld',
    props: ['imgData'],
    data() {
      return {
        img:{
          relatedFile:'',
          relatedFileName:'',
        },
      }
    },
    created() {
    },
    mounted() {
      console.log('富川',this.imgData)
      if(this.imgData.relatedFile){
        this.img.relatedFile=this.imgData.relatedFile;
        this.img.relatedFileName=this.imgData.relatedFileName;
      }
    },
    watch: {
      /*relatedFile(n,o){
        console.log(333333333,n)
        if(n){

          this.img.relatedFile=n;
          this.img.relatedFileName=this.imgData.relatedFileName;
        }
      },*/
    },
    computed:{
      /*relatedFile(){
        console.log('图欧安上传22',this.imgData.relatedFile)
        return this.imgData.relatedFile
      },*/
    },
    methods: {
      beforePicUpload(file) {
        const isJPG = file.type == 'image/jpeg' || file.type == 'image/png' || file.type == 'image/gif';

        const isLt2M = file.size / 1024 / 1024 < 2;
        if (!isJPG) {
          this.$message.warning('上传图片只能是 JPG/JPEG/PNG/GIF 格式!');
          return isJPG
        }
        if (!isLt2M) {
          this.$message.warning('上传图片大小不能超过 2MB!');
          return isLt2M
        }
        this.$message.warning('上传中');
      },
      handlePicSuccess(response, file, fileList) {
        this.$message.success('上传成功');
        this.$set(this.img,'relatedFile',response.data.fileName||response.data);
        console.log('上传success',response)
        this.$set(this.img,'relatedFileName',file.name);
        this.$emit('uploadImg',this.img);
        /*this.$refs.ruleForm.validateField('relatedFile');*/
      },
      handlePicPreview(file) { // 图片预览
      },
      handleRemove() { // 图片删除
        this.img.relatedFile='';
        this.img.relatedFileName='';
        /*this.$refs.ruleForm.validateField('relatedFile');*/
      },
      deleteIMG() {
        this.img.relatedFile = '';
        this.img.relatedFileName = '';
        /*this.$refs.ruleForm.validateField('relatedFile');*/
      },
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">

</style>
