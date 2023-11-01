<template>
  <div class="hello">
    <form @submit.prevent="insert">
      <p><input type="text" name="name" v-model="name"/></p>
      <p><input type="submit" value="저장"/></p>
    </form>

    <ul>
      <li v-for="item in data" :key="item.date">
        {{ item.name }} / {{ item.count }}
        <button @click="update(item.date, item.count)">좋아요</button>
        <button @click="del(item.date)">삭제</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  mounted(){
    this.select();
  },
  methods:{
    select(){
      axios.get("http://localhost:3000/api")
      .then(res=>{
        this.data = res.data;
      })
    },
    insert(){
      const data = {name:this.name, date:Date.now(), count:0}
      axios.post("http://localhost:3000/api/insert",data)
      .then(res=>{
        this.data = res.data;
      })
    },
    del(date){
      //여러 게시물중 date값을 찾아서 해당값만 삭제
      axios.delete(`http://localhost:3000/api/delete?date=${date}`)
      .then(res=>{
        this.data = res.data;
      })
    },
    update(date, count){
      axios.put(`http://localhost:3000/api/update?date=${date}`,{count:count+1})
      .then(res=>{
        this.data = res.data;
      })
    }
  },
  data(){
    return{ name:'', data:[] }
  },
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<style scoped>

</style>
