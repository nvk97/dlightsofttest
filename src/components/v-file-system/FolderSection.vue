<template>
  <div class="file-system__folders">
    <div class="title">Папки</div>
    <div class="folder" v-for="folder in folders.folders" :key="folder.id" @click="getFoldersChild(folder.id,folder.files)" :class="{active: activeFolderId == folder.id}">
      <folderLogo width="20px"/>
      <div class="name">{{folder.name}}</div>
    </div>
    <div class="files" v-for="file in folders.files" :key="file.id" @click="setActiveFile(file.id,file)" :class="{active:activeFileId==file.id}">
      <fileLogo/>
      <div class="name">{{file.name}}</div>
    </div>
  </div>
</template>

<script>
import folderLogo from "@/assets/svg/folder.svg"; //Импорт иконки файла
import fileLogo from "@/assets/svg/file.svg"
import eventBus from "@/eventBus.js";
export default {
  props: {
    folders: {
      default: null,
    },
    activeFolderId:{
      type:Number
    },
    activeFileId:{},
    index:{}
  },
  components: {
    folderLogo,
    fileLogo
  },
  name: "Folder",
  data() {
    return {
      
    };
  },
  methods: {
    getFoldersChild(id,files) {
      var folders = {}
      if (id == 5){
        folders = require('@/assets/child.json')
      }
      eventBus.$emit("getFoldersChild",id,files,folders,this.index); // Отправка id активной папки
    },
    setActiveFile(id,file){
      eventBus.$emit("setActiveFile",id,file)
    }
  },
};
</script>
<style lang="scss" scoped>
.file-system__folders{
  .title {
      border-bottom: 1px solid rgba(0, 0, 0, 0.2);
      height: 25px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
}
.folder,.files {
    display: flex;
    margin-left: 5px;
    cursor: pointer;
    .name {
      display: flex;
      align-items: center;
      margin-left: 5px;
    }
    &.active {
      color: #fff;
      background: crimson;
    }
  
}
</style>