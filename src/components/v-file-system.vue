<template>
  <div class="file-system">
    <FolderSection
      v-for="(folderSection, index) in folderSections"
      :key="index"
      :folders="folderSections[index]"
      :index="index"
      :activeFolderId="activeFolderId"
      :activeFileId="activeFileId"
    />
    <div class="file-system__doc-preview">
      <preview :activeFile="activeFile" v-if="!!activeFile" />
    </div>
  </div>
</template>
<script>
import FolderSection from "@/components/v-file-system/FolderSection"; //Импортирую дочерние компоненты и ивентБас
import preview from "@/components/v-file-system/preview";
import eventBus from "@/eventBus.js";
export default {
  props: ["fetchedData"], //response с сервера
  components: {
    FolderSection,
    preview,
  },
  data() {
    return {
      folderSections: [{ folders: this.fetchedData.parentsData.data }], //основной объект полученный с сервера
      activeFolderId: 0, //Иниацилизация id активной папки  и id активных файлов
      activeFileId: 0,
      activeFile: null,
    };
  },
  computed: {},
  created() {
    eventBus.$on("getFoldersChild", (id, files, folders, index) => {
      //Добавление слушателей событий из дочерних компонентов(получение id активной папки и активного файла)
      if (index > this.folderSections.length - 2) {
        this.activeFolderId = id;
        this.folderSections.push({ folders: folders.data, files: files });
      } else {
        this.activeFolderId = id;
        this.folderSections.splice(index - 1, Infinity);
      }
    });
    eventBus.$on("setActiveFile", (id, file) => {
      this.activeFileId = id;
      this.activeFile = file;
    });
  },
};
</script>

<style lang="scss" scoped>
.file-system {
  height: 100vh;
  display: flex;
  &__folders {
    width: 100%;
    height: 100%;
    flex-basis: 15%;
    border: 1px solid rgba(0, 0, 0, 0.2);
    display: flex;
    flex-direction: column;
    row-gap: 5px;
    .title {
      border-bottom: 1px solid rgba(0, 0, 0, 0.2);
      height: 25px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
  &__files {
    width: 100%;
    height: 100%;
    flex-basis: 15%;
    border: 1px solid rgba(0, 0, 0, 0.2);
    display: flex;
    flex-direction: column;
    row-gap: 5px;
    .title {
      border-bottom: 1px solid rgba(0, 0, 0, 0.2);
      height: 25px;
      display: flex;
      align-items: center;
      justify-content: center;
    }
  }
  &__doc-preview {
    flex-grow:1;
    height: 100%;
  }
}
</style>