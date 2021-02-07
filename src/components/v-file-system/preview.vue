<template>
  <div class="preview">
    <div class="preview-pdf" v-if="typeOfFile == 'pdf'">
        <iframe src="https://vk.com/doc418489921_513213568?hash=8c87a032befb7eae34&dl=f8cecf2a3ab3daba76"></iframe>
    </div>
    <div class="preview-xlsx" v-if="typeOfFile == 'xlsx'">
      <iframe class="doc-preview" :src="activeFile.link"></iframe>
    </div>
  </div>
</template>

<script>
export default {
  props:['activeFile'],
  computed:{
    typeOfFile:function(){
      let res=this.activeFile.name.split('.')
      res = res[res.length-1]
      return res
    }
  }
}
</script>


<style lang="scss" scoped>
.preview {
  height: 100%;
  &-pdf,&-xlsx{
    height: 100%;
  .doc-preview {
    width: 100%;
    height: 100%;
  }
  }
}
</style>