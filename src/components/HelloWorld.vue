<template>
<div>
  <h1>Selamat Datang</h1>
  <div>Berikut daftar kerja kita:</div>
  <ul>
    <li v-for="item in todos" :key="item.ID"><button @click="hapus(item.ID)">X</button>{{ item.TODO }}</li>
  </ul>
  <input v-model="myText"/>
  <button @click="tambahkan">Click Me</button>
</div>
</template>

<script>
import axios from 'axios'

export default{
  data : () => {
    return {
      todos:[],
      myText: ''
    }
  },
  mounted: function(){
    axios
      .get('http://localhost:3000/todo')  
      .then(result=>{
        this.todos = result.data
      })
  },
  methods: {
    tambahkan : function(){
      let newItem = {desc : this.myText}
      axios
        .post('http://localhost:3000/todo', newItem)
        .then(()=>{
          axios
            .get('http://localhost:3000/todo')
            .then(result=>{
              this.todos = result.data
              this.myText = ''
            })
        })
    },
    hapus : function(id){
      axios.delete(`http://localhost:3000/todo/${id}`)
      .then(()=>{
        axios
          .get('http://localhost:3000/todo')
          .then(result=>{
            this.todos = result.data
          })
      })
    }
  }
}
</script>