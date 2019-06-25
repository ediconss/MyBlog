<template>
    <div>
        <div id="search-header">
            <div style="position: relative;
                    background: #fff;">
          <input class="search-input" @keydown="show($event)" type="text" v-model="keyword" placeholder="搜索...">
           <button class="search-button"  @click="getPage(1)">
               <span class="el-icon-search" style="width:70px"></span>
           </button>
           </div>
            <!-- <h1 class="page-title">{{keyword}}</h1> -->
        </div>
        <div class="l-section panel" > 
            <div class="test">
                <div class="entry-grid">
                    <div class="grid-item" v-for="item in articleList" :key="item.id">
                        <div class="entry-meta">
            <a  style="color: #0e0e0e;" :href="'/topic?name='+item.topic">{{item.topic}}</a>
            <span style="float:right;color: #aaa">{{item.create_time}}</span>
          </div>
          <div>
            <a :href="'/article/'+item.id.substring(0,4)+'/'+item.id.substring(4,6)+'/'+item.id.substring(6,8)+'/'+item.id.substring(8)" >
              <img style="width:100%;height:200px" :src="item.cover"/>
            </a>
            <span style="font-size: 24px; font-weight:bold;
                    line-height: 1.4;
                    ">
              <a style="color: #0e0e0e;" :href="'/article/'+item.id.substring(0,4)+'/'+item.id.substring(4,6)+'/'+item.id.substring(6,8)+'/'+item.id.substring(8)">{{item.title}}</a>
            </span>
            <span style="display: block;
          color: #aaa;
          ">{{item.summary}}</span>
          </div>
        </div>
                    </div>
                </div>


                <div class="pagenav">
                    <a class="el-icon-d-arrow-left" @click="getPage(1)" :style="{color:this.currentPage-1!=0&&currentPage!=0?'#0e0e0e':'',cursor:this.currentPage-1!=0&&currentPage!=0?'':'default' }">  </a>
                    <a class="el-icon-arrow-left" @click="getPage(currentPage-1)" :style="{color:this.currentPage-1!=0&&currentPage!=0?'#0e0e0e':'',cursor:this.currentPage-1!=0&&currentPage!=0?'':'default'  }">  </a>                 
                    <span>   {{this.currentPage}}/{{total}}</span>             
                    <a class="el-icon-arrow-right" @click="getPage(currentPage+1)"  :style="{color:this.currentPage!=total?'#0e0e0e':'',cursor:this.currentPage!=total?'':'default' }"> </a>
                     <a class="el-icon-d-arrow-right" @click="getPage(total)"  :style="{color:this.currentPage!=total?'#0e0e0e':'',cursor:this.currentPage!=total?'':'default' }"></a>
                </div>

            </div>
   
    </div>
</template>
<script>
import articleApi from "@/api/article";
import labelApi from "@/api/label";
export default {
    data(){
        return{
        articleList:[],
        total: 0, // 总记录数
        currentPage: 1, // 当前页
       
        }

    },
     mounted(){
        // alert('as')
  },
    created(){
        //  alert('bs')
         this.keyword = this.$route.query.keyword;
         this.getPage(this.currentPage);
    },
    methods:{
        show:function(ev){
                if(ev.keyCode == 13){
                  this.$router.push({path:'/search?keyword='+this.keyword}) 
                  this.getPage(1)
                }
        },
        getPage(pageIndex){
            if(pageIndex==0||this.keyword==''){
                return
            }
            articleApi.search(pageIndex, 12, {'title':this.keyword}).then(response => {
            if(response.data.total==0){
                pageIndex=0
             }
            this.articleList = response.data.rows;
            this.total=Math.ceil(response.data.total/10);  
            if(response.flag){
                this.currentPage=pageIndex
            }
             
    });
        }
    }
}
</script>

<style>
@import "../assets/css/topic.css";
#search-header {
    margin-top: 20px;
    margin-bottom: 35px;
}
#search-header .search-button {
    font-size:200%;
    position: absolute;
    top: 0;
    right: 0;
     outline: none;
    width: 70px;
    height: 70px;
    background: none;
    border: none;
    padding: 0;
  
}
input {
  line-height: normal;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    border-radius: 0;
}
#search-header .search-input {
    font-size: 30px;
    outline: none;
    font-weight: bold;
    width: 85%;
    height: 70px;
    padding: 0 10px;
    background: none;
    border: none;
  
}


</style>
