<template>
  <div>
    <div ref="editor" style="text-align:left;margin: 5px"/>
    <div style="margin: 12px 5px;font-size: 16px;font-weight: bold;color: #696969">HTML渲染如下：</div>
    <div class="editor-content" v-html="editorContent"/>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import E from 'wangeditor'
import { getToken } from '@/utils/auth'
export default {
  props: { content: {
    type: String,
    default: '',
    required: true
  }},
  data() {
    return {
      headers: {
        'Authorization': 'Bearer ' + getToken()
      },
      editorContent: ''
    }
  },
  computed: {
    ...mapGetters([
      'imagesUploadApi'
    ])
  },
  mounted() {
    var editor = new E(this.$refs.editor)
    editor.customConfig.uploadImgShowBase64 = true // 使用 base64 保存图片
    // 不可修改
    editor.customConfig.uploadImgHeaders = this.headers
    // 自定义文件名，不可修改，修改后会上传失败
    editor.customConfig.uploadFileName = 'file'
    editor.customConfig.uploadImgServer = this.imagesUploadApi // 上传图片到服务器
    editor.customConfig.onchange = (html) => {
      this.editorContent = html
      this.$emit('msg', html)
    }
    editor.create()
    editor.txt.html(this.content) // 回显
  }
}
</script>

<style scoped>
  .editor-content{
    padding-left: 5px;
  }
</style>
