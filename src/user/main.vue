<template>
  <!-- 中间 -->
  <div style="margin-top:20px">
    <!-- 推荐栏 -->
    <section class="l-section spotlight-container">
      <!-- 走马灯 -->
        <el-carousel class="active" style="z-index:0"   @change="carouselChange" :interval="4000" arrow="always">
          <el-carousel-item  v-for="item in hotArticle" :key="item.id">
            
            <a :href="'/article/'+item.id.substring(0,4)+'/'+item.id.substring(4,6)+'/'+item.id.substring(6,8)+'/'+item.id.substring(8)" ><img :id="item.id" style="width:100%;height:100%" :src="item.cover"></a>
          </el-carousel-item>
        </el-carousel>
 

      <div class="swiper-container content-swiper" style="background: #fff;">
        <div style="margin-left: 20px;margin-top: 20px;">
         
            <a :href="'/topic?name='+carouselArticle.topic" class="topic">
              <font>{{carouselArticle.topic}}</font>
            </a>
             <span
              style="font-size:13px;font-weight:bold;color: #aaa; margin-left: 1em;"
            >{{carouselArticle.create_time}}</span>
          
           <a id="atest" :href="'/article/'+carouselArticleId.substring(0,4)+'/'+carouselArticleId.substring(4,6)+'/'+carouselArticleId.substring(6,8)+'/'+carouselArticleId.substring(8)" >
           <h1
            class="titleA"
            >{{carouselArticle.title}}</h1></a>
          <p style="color: #aaa;">{{carouselArticle.summary}}</p>
        </div>
      </div>
    </section>




    <!-- 推荐标签 -->
    <section class="tagbar" @mouseleave="stop()">
      <strong style="float: left;color:#fff;font-size:25px">话题:</strong>
      <div class="tag-list" style="z-index: 0;"  @mouseover="move($event)" @click="stop()">    
        <div  id="outer" ref='outer' class="outer" :style="{width:'6748px',left:leftsize+'px'}">
          <div class="inner">
          <a
            class="test"
            :href="'/topic?name='+item.labelName"
            v-for="item in labelList"
            :key="item.id"
          >#{{item.labelName}}&nbsp&nbsp</a>
          </div>
        </div>
        
      </div>
    </section>
    <!-- 文章 -->
    <section style="width:100%;height:80%;clear: both;
    background: #fff;">
        <header style="background: #CCCCCC;">
          <h2 class="panel-title" >最新</h2>
        </header>
       <div >
        <div class="item" v-for="item in articleList" :key="item.id">
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
       
     
    </section>
  </div>
</template>
<script>
import articleApi from "@/api/article";
import labelApi from "@/api/label";
export default {
  data() {
    return {
      carouselArticleId:"",
      Acolor: "#fff",
      fun: function() {},
      leftsize: 0,
      carouselArticle: {},
      hotArticle: [],
      labelList: [],
      rollStare:1,
      outerWidth:1,
      articleList:[]
    };
  },
  created() {
    //  this.outerWidth=this.$refs.outer.offsetHeight;
     this.roll();
    articleApi.search(1, 4, {}).then(response => {
      this.hotArticle = response.data.rows;
    });
     articleApi.getList().then(response => {
      this.articleList = response.data;
    });
    labelApi.getList().then(response => {
      this.labelList = response.data;
    });
  },
  methods: {
    roll(){
       this.fun = setInterval(() => {
       if(this.rollStare==1){
           this.leftsize = this.leftsize + 1;
       }
       if(this.rollStare==0){
           this.leftsize = this.leftsize - 1;
       }
       if(this.leftsize==window.innerWidth-parseInt(window.innerWidth*0.2)){
         this.rollStare=0
       }
       if(this.leftsize==-480){
         this.rollStare=1
       }
        }, 10);
    },
    stop() {
      clearInterval(this.fun);
    },
    move(event) {
         clearInterval(this.fun);
      if (event.pageX < 700) {
        this.fun = setInterval(() => {
          this.leftsize = this.leftsize + 1;
        }, 10);
      }
      if (event.pageX > 700) {
        this.fun = setInterval(() => {
          this.leftsize = this.leftsize - 1;
        }, 10);
      }
    },
    carouselChange: function(index) {
      this.carouselArticle = this.hotArticle[index];
      this.carouselArticleId= this.carouselArticle.id
    }
  }
};
</script>

<style >

@media (max-width: 500px) {
  .item{
margin-bottom: 10px;
  margin-top: 10px;
    padding: 0 20px;
    width: 100%;
    height: 40%;
    display: inline-block;
    vertical-align: top;
}
}
/* 4个 */
@media (min-width: 1000px) {
.item{
   margin-bottom: 30px;
   margin-top: 30px;
    padding: 0 20px;
    width: 25%;
    height: 40%;
    display: inline-block;
    vertical-align: top;
}
}
/* 3个 */
@media (max-width: 999px) and (min-width: 760px) {
.item{
margin-bottom: 25px;
  margin-top: 25px;
    padding: 0 20px;
    width: 33%;
    height: 40%;
    display: inline-block;
    vertical-align: top;
}
}
/* 2个 */
@media (max-width: 760px) and (min-width: 500px) {
.item{
margin-bottom: 20px;
  margin-top: 20px;
    padding: 0 20px;
    width: 50%;
    height: 40%;
    display: inline-block;
    vertical-align: top;
}
}

.item .entry-meta {
    height: 20;
    margin-bottom: 3px;
    font-size: 12px;
    font-weight: bold;

}

.panel-title {
    width: 50px;
    background: #0e0e0e;
    /* line-height: 28px; */
    padding: 0 7px;
    font-size: 1em;
    color: #fff;
}




.tagbar {
  margin-bottom: 20px;
  position: relative;
  background: #0e0e0e;
  font-size: 28px;
  color: #fff;
  font-weight: bold;
  padding: 8px 10px;
  line-height: 1.2;
}
.tagbar .tag-list {
  position: relative;
  overflow: hidden;
  white-space: nowrap;
}
.tagbar .tag-list .outer {
  position: relative;
  left: 0;
  top: 0;
  width: 500%;
  z-index: 1;
}
.tagbar .tag-list .inner {
  display: inline;
}
.tagbar .tag-list:after {
  position: absolute;
  pointer-events: none;
  content: "";
  left: 0;
  top: 0;
  height: 100%;
  width: 5em;
  /* background: -webkit-gradient(linear, right top, left top, from(rgba(14,14,14,0)), to(#0e0e0e)); */
  background: -webkit-linear-gradient(right, rgba(14, 14, 14, 0), #0e0e0e);
  /* background: -o-linear-gradient(right, rgba(14,14,14,0), #0e0e0e); */
  /* background: #fff; */
  z-index: 2;
  opacity: 1;
  -webkit-transition: opacity 0.3s;
  -o-transition: opacity 0.3s;
  transition: opacity 0.3s;
}
.tagbar .tag-list:before {
  pointer-events: none;
  content: "";
  position: absolute;
  right: 0;
  top: 0;
  height: 100%;
  width: 5em;
  /* background: -moz-linear-gradient(linear, left top, right top, from(rgba(14,14,14,0)), to(#0e0e0e)); */
  background: -webkit-linear-gradient(left, rgba(14, 14, 14, 0), #0e0e0e);
  /* background: -o-linear-gradient(left, rgba(14,14,14,0), #0e0e0e); */
  /* background: linear-gradient(to right, rgba(14,14,14,0), #0e0e0e); */
  z-index: 2;
  opacity: 1;
  -webkit-transition: opacity 0.3s;
  -o-transition: opacity 0.3s;
  transition: opacity 0.3s;
}
/* a.test3:hover,
:visited {
  color: #0e0e0e;
} */
a.test:hover,
:visited {
  color: #bbb;
}
a.topic:hover,
:visited {
  color: #fff;
}


.topic {
  font-size: 12px;
  display: inline-block;
  background: #0e0e0e;
  color: #fff;
  padding: 3px 15px;
}
.el-carousel__item h3 {
  color: #475669;
  font-size: 18px;
  opacity: 0.75;
  line-height: 300px;
  margin: 0;
  height: 100%;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;

}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}

@media (max-width: 500px) {
.container {
  background: #F1F1F1;
  margin-right: 20px;
}
}
/* 4个 */
@media (min-width: 1000px) {
.container {
  background: #F1F1F1;
  margin-right: 60px;
}
}
/* 3个 */
@media (max-width: 999px) and (min-width: 760px) {
.container {
  background: #F1F1F1;
  margin-right: 20px;
}
}
/* 2个 */
@media (max-width: 760px) and (min-width: 500px) {
.container {
  background: #F1F1F1;
  margin-right: 10px;
}
}
@media (max-width: 920px){
.spotlight-container .content-swiper {
    position: static;
    width: auto;
    height: auto;
}
.titleA{
  font-size:150%;
  line-height: 1.2;
  margin-bottom: 0.6em;
}
 .l-section {
    margin-bottom: 30px;
    clear: both;
}
.el-carousel__container {
    position: relative;
    height: 300px;
}
}
@media (min-width: 920px){
  .el-carousel__container {
    position: relative;
    height: 500px;
}
  .active{
    width:75%
  }
  .l-section {
    margin-bottom: 30px;
    clear: both;
}
  .titleA{
  font-size:200%;
  line-height: 1.2;
  margin-bottom: 0.6em;
}
.spotlight-container .content-swiper {
  position: absolute;
  top: 0;
  right: 0;
  width: 25%;
  height: 100%;
}
}
.swiper-container {
  margin: 0 auto;
  position: relative;
  overflow: hidden;
  /* z-index: 1; */
}
.spotlight-container {
  position: relative;
  overflow: hidden;
}

</style>
