<template>
  <div class="header">
    <router-link :to="{name:'root'}">
        <img src="../assets/cnodejs_light.svg" alt="" @click="home">
    </router-link>
    <ul>
        <li><a href="#" @click="home">首页</a></li>
        <li><a href="#">新手入门</a></li>
        <li><a href="#">API</a></li>
        <li><a href="#">关于</a></li>
        <li><a href="#">注册</a></li>
        <li><a href="#">登录</a></li>
    </ul>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name: 'Header',
  methods:{
      home(){
          location.reload()
      }
  }
}
</script>

<style scoped>
    .header{
        background-color:#444;
        height:50px;
    }
    img{
        max-width:120px;
        margin-left:82px;
        margin-top:10px;
    }
    ul{
        list-style:none;
        float:right;
        margin:4px 80px 4px 4px;
    }
    li{
        display:inline-block;
        padding:10px 12px;
    }
    a{
        text-decoration:none;
        color:#ccc;
        font-size:14px;
    }
</style>