<template>
  <div class="PostList">
    <div v-if="isLoading" class="loading">正在加载中...</div>
    <div class="posts" v-else>
        <ul>
            <li>
                <div class="topbar">
                    <span @click="all" :class="{active:allisActive}">全部</span>
                    <span @click="good" :class="{active:goodisActive}">精华</span>
                    <span @click="share" :class="{active:shareisActive}">分享</span>
                    <span @click="ask" :class="{active:askisActive}">问答</span>
                    <span @click="job" :class="{active:jobisActive}">招聘</span>
                </div>
            </li>
            <li v-for="post in posts">
                <router-link :to="{name:'user_info',params:{name:post.author.loginname}}">
                    <img :src="post.author.avatar_url" alt="">
                </router-link> 
                <span class="allcount">
                    <span class="reply_count">{{post.reply_count}}</span>/{{post.visit_count}}
                </span>
                <span :class="{put_good:(post.good==true),put_top:(post.top==true),
                  'topiclist-tab':(post.good!=true&&post.top!=true)}">
                    <span>{{post|tabFormatter}}</span>
                </span>
                <router-link :to="{name:'post_content',params:{id:post.id,name:post.author.loginname}}">
                    <span>{{post.title}}</span>
                </router-link>
                <span class="last_reply">{{post.last_reply_at|formatDate}}</span>
            </li>
            <li>
                <Pagination @handleList="renderList" ref="a"></Pagination>
            </li>          
        </ul>
    </div>
    
  </div>
</template>

<script>
import Pagination from './Pagination'

export default {
  name: 'PostList',
  data(){
      return {
        isLoading:false,
        posts:[],
        postpage:1,
        tab:'all',
        goodclicked:false,
        goodisActive:'',
        shareisActive:'',
        shareclicked:false,
        askisActive:'',
        askclicked:false,
        jobisActive:'',
        jobclicked:false,
        allisActive:true    
      }
  },
  components:{Pagination},
  methods: {
        getData(){
            this.$http.get('https://cnodejs.org/api/v1/topics',{params:{page:this.postpage,limit:15}})
                .then((res)=>{
                    this.isLoading=false
                    this.posts=res.data.data
                    console.log(res)
                })
                .catch((err)=>{
                    console.log(err)
                })
        },
        getgood(){
            this.$http.get('https://cnodejs.org/api/v1/topics',{params:{page:this.postpage,limit:15,tab:'good'}})
                .then((res)=>{
                    this.isLoading=false
                    this.posts=res.data.data
                    // console.log(res)
                })
                .catch((err)=>{
                    console.log(err)
                })
        },
        getshare(){
            this.$http.get('https://cnodejs.org/api/v1/topics',{params:{page:this.postpage,limit:15,tab:'share'}})
            .then((res)=>{
                    this.isLoading=false
                    this.posts=res.data.data
                    console.log(res)
                })
                .catch((err)=>{
                    console.log(err)
                })
        },
        getask(){
            this.$http.get('https://cnodejs.org/api/v1/topics',{params:{page:this.postpage,limit:15,tab:'ask'}})
            .then((res)=>{
                    this.isLoading=false
                    this.posts=res.data.data
                    console.log(res)
                })
                .catch((err)=>{
                    console.log(err)
                })
        },
        getjob(){
            this.$http.get('https://cnodejs.org/api/v1/topics',{params:{page:this.postpage,limit:15,tab:'job'}})
            .then((res)=>{
                    this.isLoading=false
                    this.posts=res.data.data
                    console.log(res)
                })
                .catch((err)=>{
                    console.log(err)
                })
        },
        renderList(value){
            this.postpage = value;
            if(this.tab=='good'){
                this.getgood()
                if(this.goodclicked==false){
                    this.$refs.a.changepage(value)
                    this.goodclicked=true
                }  
            }else if(this.tab=='share'){
                this.getshare()
                if(this.shareclicked==false){
                    this.$refs.a.changepage(value)
                    this.shareclicked=true
                } 
            }else if(this.tab=='ask'){
                this.getask()
                if(this.askclicked==false){
                    this.$refs.a.changepage(value)
                    this.askclicked=true
                } 
            }else if(this.tab=='job'){
                this.getjob()
                if(this.jobclicked==false){
                    this.$refs.a.changepage(value)
                    this.jobclicked=true
                } 
            }else{
                this.getData()
            }
        },
        good(){
            this.tab='good'
            this.goodclicked=false
            this.renderList(1)
            this.goodisActive=true
            this.shareisActive=false
            this.askisActive=false
            this.jobisActive=false
            this.allisActive=false
        },
        share(){
            this.tab='share'
            this.shareclicked=false
            this.renderList(1)
            this.shareisActive=true
            this.goodisActive=false
            this.askisActive=false
            this.jobisActive=false
            this.allisActive=false
        },
        ask(){
            this.tab='ask'
            this.askclicked=false
            this.renderList(1)
            this.askisActive=true
            this.goodisActive=false
            this.shareisActive=false
            this.jobisActive=false
            this.allisActive=false
        },
        job(){
            this.tab='job'
            this.jobclicked=false
            this.renderList(1)
            this.jobisActive=true
            this.goodisActive=false
            this.shareisActive=false
            this.askisActive=false
            this.allisActive=false
        },
        all(){
            location.reload()
            this.allisActive=true
            this.shareisActive=false
            this.goodisActive=false
            this.askisActive=false
            this.jobisActive=false
        }      
  },
  beforeMount(){
      this.isLoading=true
      this.getData()
  }
}
</script>

<style scoped>
   
    .PostList{
        background-color: #e1e1e1;
    }
    .posts {
        margin-top: 10px;
    }
    img{
        width:30px;
        height:30px;
        vertical-align: middle;
    }
    ul {
        list-style: none;
        width: 100%;
        margin: 0 auto;
    }
    ul li:not(:first-child) {
        padding: 9px;
        font-size: 15px;
        font-family: "Helvetica Neue", "Luxi Sans", "DejaVu Sans", Tahoma, "Hiragino Sans GB", STHeiti, sans-serif !important;
        font-weight: 400;
        background-color: white;
        color: #333;
        border-top: 1px solid #f0f0f0;
        
    }
    li:not(:first-child):hover {
        background: #f5f5f5;;
    }
    li span {
        line-height: 30px;
    }
    .allcount {
        width: 70px;
        display: inline-block;
        text-align: center;
        font-size: 12px;
    }
    .reply_count {
        color: #9e78c0;
        font-size: 14px;
    }
    .put_good, .put_top {
        background: #80bd01;
        padding: 2px 4px;
        border-radius: 3px;
        -webkit-border-radius: 3px;
        -moz-border-radius: 3px;
        -o-border-radius: 3px;
        color: #fff;
        font-size: 12px;
        margin-right: 10px;
    }
    .topiclist-tab {
    background-color: #e5e5e5;
    color: #999;
    padding: 2px 4px;
    border-radius: 3px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    -o-border-radius: 3px;
    font-size: 12px;
    margin-right: 10px;
  }
  .last_reply {
    text-align: right;
    min-width: 50px;
    display: inline-block;
    white-space: nowrap;
    float: right;
    color: #778087;
    font-size: 12px;
  }
  .topbar {
    height: 40px;
    background-color: #f5f5f5;
    padding:0 10px;
  }
  .topbar span {
    font-size: 14px;
    color: #80bd01;
    line-height: 40px;
    margin: 0 10px;
    padding:2px 4px;
    cursor: pointer;
    border-radius: 3px;
  }
  .topbar span:hover {
    color: #9e78c0;
  }
  .loading {
    text-align: center;
  }
  a {
    text-decoration: none;
    color: black;
  }

  a:hover {
    text-decoration: underline;
  }
  span.active{
      color: #fff;
      background-color: #80bd01;
  }
</style>