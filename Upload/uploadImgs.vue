<template>
  <!--图片集-->
  <div class="uploadImgs">
    <div class="imgCss" v-if="imgData.vueStatus">
      <img :src="img.url" alt="" v-for="(img,i) in imgList" :key="i" v-if="imgList.length>0">
      <div v-if="imgList.length==0" style="color:#606266">暂无文件</div>
    </div>
    <el-upload
      class="upload-demo"
      :action="imgData.action"
      :before-upload="beforeImgsUpload" :on-success="handleImgsSuccess"
      :on-remove="handleImgsRemove"
      list-type="picture" :file-list="imgList" v-if="!imgData.vueStatus">
      <el-button size="" type="primary" :loading="uploadingImgs">点击上传</el-button>
    </el-upload>

    <div class="el-upload__tip" v-if="!imgData.vueStatus">仅支持jpg、jpeg、png格式，单张不超过10M</div>
  </div>
</template>

<script>
  import './index.css';
  export default {
    name: 'HelloWorld',
    props: ['imgData'],
    data() {
      return {
        uploadingImgs:false,
        imgList:[],
      }
    },

    created() {
    },
    mounted() {
      if(this.imgData.imgs.length>0){
        this.imgData.imgs.forEach((item,index)=>{
          let imgLink={url:`${this.imgData.uploadUrl}file/?fileName=${item}&isOnLine=true`,response:item};
          this.imgList=[...this.imgList,imgLink];
        })
      }
    },
    watch: {},
    methods: {
      beforeImgsUpload(file){
        const isJPG = file.type == 'image/jpeg' || file.type == 'image/png';
        const isLt10M = file.size / 1024 / 1024 < 10;
        if (!isJPG) {
          this.$message.error('上传图片只能是 JPG/JPEG/PNG 格式!');
          return isJPG
        }
        if (!isLt10M) {
          this.$message.error('上传图片大小不能超过 10MB!');
          return isLt10M
        }
        this.uploadingImgs=true;
      },
      handleImgsSuccess(response, file, fileList){
        this.$message.success('上传成功');
        this.imgList=fileList;
        this.uploadingImgs=false;
        this.$emit('uploadImg',this.imgList);
      },
      handleImgsRemove(file, fileList){
        console.log('移除图片集',file, fileList)
        this.imgList=fileList;
        this.$emit('uploadImg',this.imgList);
      },
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
