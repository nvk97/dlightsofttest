<template>
  <main class="main">
    <vFileSystem :fetchedData="fetchedData"/>
  </main>
</template>

<script>
// import axios from 'axios'
import vFileSystem from '@/components/v-file-system.vue'
export default {
  name: 'Home',
  data(){
    return{
      fetchedData:{
        parentsData:{},
      }
    }
  },
  components: {
    vFileSystem
  },
  created(){
    // axios.get('http://someapi.com/files/documents?user-id=1234') Запрос на сервер
    // .then(res =>{
    //   this.parentsData = res.data.parents,
    //   this.childData = res.data.child
    // })
    // .catch(e=>this.$store.someErrorHandler.push(e))
    // fetch('http://someapi.com/files/documents?user-id=1234') Альтернативный запрос
    // .then(res => res = res.json())
    // .then(json => this.datas = json)
    this.fetchedData.parentsData = require('@/assets/parents.json')
   
  }
}
</script>
