<template>
  <div class="file-system">
    <FolderSection
      v-for="(folderSection, index) in folderSections"
      :key="index"
      :folders="folderSections[index]"
      :index="index"
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
      folderSections: [
        {
          folders: this.fetchedData.parentsData.data,
          activeId: null,
          name: "Папки",
        },
      ], //основной объект полученный с сервера
      activeFileId: null,
      activeFile: null,
    };
  },
  computed: {},
  created() {
    eventBus.$on("getFoldersChild", (id, files, folders, index) => {
      //Добавление слушателей событий из дочерних компонентов(получение id активной папки и активного файла)
      if (index > this.folderSections.length - 2) {
        this.folderSections.push({
          folders: folders.data,
          files: files,
          activeId: null,
        });
        this.$set(this.folderSections[index], "activeId", id);
      } else if (this.folderSections[index].activeId == id) {
        this.folderSections.splice(index + 1, Infinity);
        this.$set(this.folderSections[index], "activeId", null);
      } else {
        this.folderSections.splice(index + 1, Infinity);
        this.$set(this.folderSections[index], "activeId", null);
        this.folderSections.push({
          folders: folders.data,
          files: files,
          activeId: null,
        });
        this.$set(this.folderSections[index], "activeId", id);
      }
    });
    eventBus.$on("setActiveFile", (id, file) => {
      if (this.activeFileId == id) {
        this.activeFileId = null
        this.activeFile = null
      } else {
        this.activeFileId = id;
        this.activeFile = file;
      }
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
    flex-grow: 1;
    height: 100%;
  }
}
</style>