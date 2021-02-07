<template>
    <div class="file" @click="setActiveFileId" :class="{active:file.id == activeFileId}">
        <fileSvg/>
        <div class="file-name">{{file.name}}</div>
    </div>
</template>
<script>
import fileSvg from '@/assets/svg/file.svg'
import eventBus from '@/eventBus.js'
 export default {
     props:['file','activeFileId'],
     components:{
         fileSvg
     },
     methods:{
         setActiveFileId(){
             eventBus.$emit('setActiveFileId',this.file.id)
         }
     }
 }
 </script>

 <style lang="scss" scoped>
 .file{
    cursor: pointer;
    display: flex;
    align-items: center;
    margin-left: 5px;
    height:30px;
    &-name{
        margin-left: 5px;
    }
    &.active{
      color: #fff;
      background: crimson;
    }
 }
 </style>
