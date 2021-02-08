<template>
  <div class="preview">
    <div class="preview-pdf" v-if="typeOfFile == 'pdf'">
      <embed class="doc-preview" type="application/pdf" :src="activeFile.link">
    </div>
    <div class="preview-xlsx" v-if="typeOfFile == 'xlsx'">
      <iframe class="doc-preview" :src="activeFile.link"></iframe>
    </div>
  </div>
</template>

<script>
/*
Я не смог найти исчерпывающей информации по встраиванию pdf(Локального)
Вебпак не мог найти его(Я создавал правила file-loader для pdf и вставлял его через require('@/assets/files/file.pdf') результат 0)
https://forum.vuejs.org/t/how-to-load-a-pdf-file-in-the-vue-project-generated-with-vue-cli3/58157/2
Но меня не покидает ощущение что я что-то сделал не так.

xlsx насколько я понял вообще нативно невозможно вставить(возможно есть какие-то библиотеки) поэтому я просто вставил ссылки на google doc через iframe
Мои решения мне кажутся не особо правильными и хотелось бы уточнить как стоит реализовывать preview файлов
Мое представление что на стороне бека должны быть созданы картинки или pdf файлы которые можно использовать в качестве превью

*/
export default {
  props:['activeFile'],
  computed:{
    typeOfFile:function(){ // определение типа файла
      let res=this.activeFile.name.split('.')
      res = res[res.length-1]
      return res
    }
  },
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