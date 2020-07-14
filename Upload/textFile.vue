<template>
  <!-- 上传文件-->
  <div>
    上传文件
    <uploadFile :fileData="fileData" @uploadFile="uploadFile"></uploadFile>
  </div>
</template>

<script>
  import uploadFile from '../components/Upload/uploadFile';
  export default {
    name: 'HelloWorld',
    data() {
      return {
        fileData:{
          vueStatus:false,//页面是不是查看页面
          action:`http://192.168.2.174:8295/file/file/upload`,//上传接口
          uploadUrl:'http://192.168.2.174:8295/file/file/',//域名
          relatedFile:'e5d7aa3f19c740caa8d88229223b1af1',//一个文件时 文件id
          relatedFileName:'测试文件名',//一个文件时 文件名
          limit:1,
        }
      }
    },
    components:{uploadFile},
    created() {
    },
    watch: {},
    methods: {
      uploadFile(file){
        console.log('上传突破',file)
      },
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
