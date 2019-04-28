<template>
  <div id="App">
    <Header/>
    <div class="main">
      <router-view name="slidebar"></router-view>
      <router-view name="main"></router-view>
    </div>   
  </div>
</template>

<script>
import Header from './components/Header'


export default {
  name: 'App',
  components:{Header}
}
</script>

<style>
*{
    margin:0;
    padding:0;
}
.main{
  width:88%;
  margin:0 auto;
}
html{
  background-color:#e1e1e1;
}

</style>
