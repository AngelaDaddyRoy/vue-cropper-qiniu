<template>
  <div id="app">
    <div style="width:100px;height:100px;" id="preview"></div>
    <img ref="image" src="./assets/logo.png">
    <div style="background:#ccc" alt="">
      <img :src='base64Preview' />
    </div>
    <button @click="crop">crop</button>
    <button @click="getCroppedImage">getCroppedImage</button>
    <button @click="uplaodQiniu">uplaodQiniu</button>
  
  </div>
</template>

<script>
import Cropper from 'cropperjs'

export default {
  name: 'app',
  data() {
    return {
      cropper: null,
      base64Preview: '',
      croppedBlob: null
    }
  },
  methods: {
    crop() {
      this.cropper = new Cropper(this.$refs.image, {
        viewMode: 1,
        dragMode: 'move',
        preview: '#preview',
        autoCropArea: 1,
      });
    },
    getCroppedImage() {
      var re = this.cropper.getCroppedCanvas({
        width: 160,
        height: 90,
        fillColor: '#fff',
        imageSmoothingEnabled: false,
        imageSmoothingQuality: 'high',
      })
      console.log(re);
      this.base64Preview = re.toDataURL();
      re.toBlob(x => {
        this.croppedBlob = x;
        console.log(this.croppedBlob);
      })
    },
    uplaodQiniu() {

      var pic = this.base64Preview.replace(/^.*?,/, '');
      var url = `http://upload.qiniu.com/putb64/-1/key/testimage.png`; //非华东空间需要根据注意事项 1 修改上传域名
      var xhr = new XMLHttpRequest();
      xhr.open("POST", url, true);
      xhr.setRequestHeader("Content-Type", "application/octet-stream");
      xhr.setRequestHeader("Authorization", "UpToken LnilfBJjxDf2jkESbg1I3gMeT1FPreMWhTEdkElq:pneBCfdvuLPwVo-F08ilSxckZZ0=:eyJjYWxsYmFja0JvZHlUeXBlIjoiYXBwbGljYXRpb24vanNvbiIsInNjb3BlIjoiYmlvdnI6dGVzdGltYWdlLnBuZyIsImRlYWRsaW5lIjoxNTAxNjQ0NDEzLCJjYWxsYmFja0JvZHkiOiJ7XCJrZXlcIjpcIiQoa2V5KVwiLFwiaGFzaFwiOlwiJChldGFnKVwiLFwiYnVja2V0XCI6XCIkKGJ1Y2tldClcIixcImZzaXplXCI6JChmc2l6ZSl9In0=");
      xhr.onload = function (oEvent) {
        if (xhr.status == 200) {
          console.log('success')
        } else {
          console.log("Error " + xhr.status + " occurred uploading your file.<br \/>");
        }
      };
      xhr.send(pic);

      // var formData = new FormData();
      // formData.append('file', this.croppedBlob);
      // formData.append('key', 'testimage.png');
      // formData.append('token', 'LnilfBJjxDf2jkESbg1I3gMeT1FPreMWhTEdkElq:pneBCfdvuLPwVo-F08ilSxckZZ0=:eyJjYWxsYmFja0JvZHlUeXBlIjoiYXBwbGljYXRpb24vanNvbiIsInNjb3BlIjoiYmlvdnI6dGVzdGltYWdlLnBuZyIsImRlYWRsaW5lIjoxNTAxNjQ0NDEzLCJjYWxsYmFja0JvZHkiOiJ7XCJrZXlcIjpcIiQoa2V5KVwiLFwiaGFzaFwiOlwiJChldGFnKVwiLFwiYnVja2V0XCI6XCIkKGJ1Y2tldClcIixcImZzaXplXCI6JChmc2l6ZSl9In0=');
      // var oReq = new XMLHttpRequest();
      // oReq.open("POST", "http://upload.qiniu.com/");
      // oReq.onload = function (oEvent) {
      //   if (oReq.status == 200) {
      //     console.log('success')
      //   } else {
      //      console.log("Error " + oReq.status + " occurred uploading your file.<br \/>");
      //   }
      // };
      // oReq.send(formData);

    }
  }
}
</script>

<style>

</style>
