<template>
  <div class="file-system">
    <div class="file-system__folders">
      <div class="title">Папка</div>
      <Folder
        v-for="folder in folders"
        :key="folder.id"
        :folder="folder"
        :activeFolderId="activeFolderId"
      />
    </div>
    <div class="file-system__files">
      <div class="title">Файл</div>
      <Files
        v-for="file in activeFolderFiles"
        :key="file.id"
        :file="file"
        :activeFileId="activeFileId"
      />
    </div>
    <div class="file-system__doc-preview">
      <Preview :activeFile="activeFile" v-if="!!activeFile"/>
    </div>
  </div>
</template>
<script>
import Folder from "@/components/v-file-system/folders";
import Files from "@/components/v-file-system/files";
import Preview from "@/components/v-file-system/preview";
import eventBus from "@/eventBus.js";
export default {
  props: ["fetchedData"],
  components: {
    Folder,
    Files,
    Preview,
  },
  data() {
    return {
      folders: this.fetchedData.parentsData.data,
      activeFolderId: 0,
      activeFileId: 0,
    };
  },
  computed: {
    activeFolderFiles: function () {
      function getActiveFolder(currentFolder, id) {
        if (currentFolder.id == id && !!currentFolder.files.length) {
          return currentFolder.files;
        } else if (currentFolder.under_folder) {
          for (let i = 0; i < currentFolder.under_folder.length; i++) {
            var res = getActiveFolder(currentFolder.under_folder[i], id);
            if (res) {
              break;
            }
          }
        }
        return res || undefined;
      }
      var res;
      for (let i = 0; i < this.folders.length; i++) {
        res = getActiveFolder(this.folders[i], this.activeFolderId);
        if (res) {
          break;
        }
      }
      return res || {};
    },
    activeFile: function () {
      var res
      for (let i = 0; i < this.activeFolderFiles.length; i++) {
        if (this.activeFolderFiles[i].id == this.activeFileId) {
           res =  this.activeFolderFiles[i];
        }
       
      } 
      return res||undefined;
    },
  },
  created() {
    eventBus.$on("setActiveFolderId", (id) => {
      this.activeFolderId = id;
    });
    eventBus.$on("setActiveFileId", (id) => {
      this.activeFileId = id;
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
    flex-basis: 70%;
    height: 100%;
  }
}
</style>