<template>
  <!-- 上传图片-->
  <div>
    上传图片
    <uploadImg :imgData="imgData" @uploadImg="uploadImg"></uploadImg>
  </div>
</template>

<script>
  import uploadImg from './uploadImg';
  export default {
    name: 'HelloWorld',
    data() {
      return {
        imgData:{
          vueStatus:false,//页面是不是查看页面
          action:`http://192.168.2.174:8295/file/file/upload/info`,//上传接口
          uploadUrl:'http://192.168.2.174:8295/file/file/',//域名
          relatedFile:'',//一个图片时 图片id
          relatedFileName:'',//一个图片时 图片名
          imgType:'',
          imgs:[],
        }
      }
    },
    components:{uploadImg},
    created() {
    },
    watch: {},
    methods: {
      uploadImg(file){
        console.log('上传突破',file)
      },
    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
