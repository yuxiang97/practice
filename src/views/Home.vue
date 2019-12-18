<template>
  <div class="home">
    <ul class="list">
      <li class="li"
          v-for="(item,index) in list"
          :key="index"
      >
        <router-link to="/about">{{item.title}}</router-link >
        <span class="fa fa-close" @click="del(item.id)"></span>
      </li>
    </ul>
    <div class="input">
      <input type="text" placeholder="请输入需要创建的名称">
      <button @click="add()">创建</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
const getList = () => {
  axios({
    method: 'GET',
    url: 'http://localhost:3000/todo'
  }).then(res => {
    const {data} = res
    document.querySelector('.list').innerHTML = ''
    data.map(item => {
      const {id,title} = item
      document.querySelector('.list').innerHTML += `
      <li class="li"
          v-for="(item,index) in list"
          :key="index"
      >
        <router-link to="/about" :id = ${id}>${title}</router-link>
        <span class="fa fa-close" @click="del(${id})"></span>
      </li>
      `
    })
  })
};
export default {
  name: 'home',
  data() {
    return {
      list:[]
    }
  },
  created() {
    axios({
      method: 'GET',
      url: 'http://localhost:3000/todo'
    }).then(res => {
      this.list = res.data
    })
  },
  methods:{
    del(id){
      if(window.confirm('确定要删除此项吗？')){
        axios({
          url: `http://localhost:3000/todo/${id}`,
          method: 'DELETE',
        }).then(()=> {
            getList()
        })
      }
    },
    add(){
      let content = document.querySelector('.input input[type=text]')
      // console.log(content)
      if (content.value !== '') {
        // console.log(content.value)
        axios({
          method: 'POST',
          url: 'http://localhost:3000/todo',
          data: {
            title:content.value
          }
        }).then(()=> {
          content.value = ''
          getList()
        })
      }else {
        alert('名称不能为空')
      }
    }
  }
}
</script>
<style lang="scss">
  .home {
    text-align: left;
    ul {
      margin: 0;
      padding: 0;
        .li {
          display: flex;
          width: 300px;
          align-items: center;
          justify-content: space-between;
          height: 30px;
          border: 1px solid;
          padding: 0 10px;
          box-sizing: border-box;
        }
    }
    .input {
      margin-top: 10px;
    }
  }

</style>

