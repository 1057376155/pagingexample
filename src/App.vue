<template>
  <div id="app">
    <ul>
      <li v-for="(item,index) in list" :key="index">
        <span>{{item.userName}}</span> - <span>{{item.address}}</span>
      </li>
    </ul>
    <Pagination @change="getList" :defaultIndex="defaultIndex" :pageSize="this.pageSize" :total="this.total"></Pagination>
  </div>
</template>

<script>
import Pagination from './components/Pagination.vue'
import axios from 'axios'
export default {
  name: 'App',
  components: {
    Pagination
  },
  data(){
    return{
      list:[],
      pageSize:10,
      total:0,
      defaultIndex:6
    }
  },
  mounted(){
   this.getList(this.defaultIndex)
  },
  methods:{
    change(index){
     this.getList(index)
    },
    getList(index){
      axios.get(`http://yapi.luckly-mjw.cn/mock/50/test/users?pageIndex=${index}`).then((r)=>{
          if(r.data.code==200){
            this.list=r.data.data.list
            this.total=parseInt(r.data.data.page.itemSum)
          }
        })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
