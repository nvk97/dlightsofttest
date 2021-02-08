<template>
  <div class="folder">
    <div class="folder-parent" @click="[displaySubFolders(),setActiveFolderId()]" :class='{active: activeFolderId == folder.id}'>
      <folderLogo width="20px" />
      <div class="folder-parent-name">{{ folderName }}</div>
    </div>
    <div class="folder-childs" v-if="subFoldersVisible">
      <Folder v-for="folder in subFolders" :key="folder.id" :folder="folder" :activeFolderId="activeFolderId"/> 
      <!-- Рекурсивный компонент для отоброжения вложенности папок -->
    </div>
  </div>
</template>

<script>
import folderLogo from "@/assets/svg/folder.svg";//Импорт иконки файла
import eventBus from '@/eventBus.js'
export default {
  props: {
    folder: {
      type: Object,
      default: null,
    },
    activeFolderId:{
      type:Number
    }
  },
  components: {
    folderLogo,
  },
  name: "Folder",
  data() {
    return {
      folderName: this.folder.name,
      subFolders: this.folder.under_folder,
      subFoldersVisible:false,
      hasChilds:!!this.folder.under_folder
    };
  },
  methods:{
      displaySubFolders(){ //Скрытие и отображение дочерних папок
        if (this.hasChilds){
          this.subFoldersVisible =  this.subFoldersVisible?false:true
        }
      },
      setActiveFolderId(){
        eventBus.$emit('setActiveFolderId',this.folder.id)// Отправка id активной папки
      }

  },
};
</script>
<style lang="scss" scoped>
.folder {
  &-parent {
    display: flex;
    margin-left: 5px;
    cursor: pointer;
    &-name {
      display: flex;
      align-items: center;
      margin-left: 5px;
    }
    &.active{
      color: #fff;
      background: crimson;
    }
  }
  &-childs{
    display: flex;
    flex-direction: column;
    row-gap: 5px;
    margin: 5px 0 0 5px;
  }
}
</style>